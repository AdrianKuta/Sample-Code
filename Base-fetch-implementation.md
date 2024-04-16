# Base fetch implementation

```Typescript
import deepmerge from "deepmerge"
import type { Session } from "next-auth"
import { v4 as uuidv4 } from "uuid"

import { getServerSession } from "@/lib/auth"
import logger from "@/lib/logger"
import { transformKeysToSnakeCase } from "@/lib/utils"

export type MovedPermanentlyErrorType = {
  response: JSONType
  code: "MOVED_PERMANENTLY"
}

export type NotFoundErrorType = {
  response: JSONType
  code: "ELEMENT_NOT_FOUND"
}
export type InvalidRequestErrorType = {
  response: JSONType
  code: "REQUEST_INVALID"
}

export type BaseError =
  | MovedPermanentlyErrorType
  | InvalidRequestErrorType
  | NotFoundErrorType

type JSONType = { [key: string]: any }

export const ERROR_DICTIONARY: Record<
  number,
  "MOVED_PERMANENTLY" | "REQUEST_INVALID" | "ELEMENT_NOT_FOUND" | undefined
> = {
  301: "MOVED_PERMANENTLY",
  400: "REQUEST_INVALID",
  404: "ELEMENT_NOT_FOUND",
}

/**
 * @throws Error {Unexpected error while fetching data ....}
 * @throws Not OK response AND different than 4XX status {Unexpected error while fetching data: ...}
 */
export async function baseFetch<T>(
  url: string,
  options?: RequestInit
): Promise<T | BaseError | never> {
  const session =
    typeof global.window !== "undefined" ? undefined : await getServerSession()
  const method = options?.method ?? "GET"
  const body = options?.body
  const fetchOptions = deepmerge(
    {
      method,
      headers: {
        ...(body !== undefined && { "Content-Type": "application/json" }),
        ...(session && {
          authorization: `Bearer ${
            //@ts-ignore
            (session as Session).user.accessToken
          }`,
        }),
      },
    },
    options ?? {}
  )
  const requestId = uuidv4()

  let response: Response
  let responseText: any
  let responseJson: (T & { etag?: string | null }) | string | JSONType

  let responseData: JSONType | string

  try {
    logger.logRequestInit(requestId, {
      url,
      headers: fetchOptions.headers,
      method: fetchOptions.method,
    })
    response = await fetch(url, fetchOptions)

    responseText = await response.text()

    const isJSONRes = response.headers
      ?.get("content-type")
      ?.includes("application/json")
    // JSON: parse it and set etag

    if (isJSONRes && responseText !== "") {
      responseJson = JSON.parse(responseText)

      const etag = response.headers?.get("etag")
      if (etag && typeof responseJson === "object") {
        responseJson.etag = etag
      }

      responseData = transformKeysToSnakeCase(responseJson)
    }
    // response is treated as text
    else {
      responseData = responseText
    }

    logger.logResponseReceived(requestId, response.status, responseData)
  } catch (error) {
    logger.logResponseError(requestId, error)
    logger.logUnexpectedResponseContentType(requestId, responseText)

    throw new Error("Unexpected error while fetching data: " + error)
  }

  if (!response.ok) {
    const errorMessage = {
      status: response.status,
      statusText: response.statusText,
    }
    logger.logResponseNotOk(requestId, {
      ...errorMessage,
      data: responseData,
      headers: fetchOptions.headers,
      method: fetchOptions.method,
      url,
    })

    const handledError = ERROR_DICTIONARY[response.status]

    if (handledError) {
      return {
        response: responseData,
        code: handledError,
      } as BaseError
    }

    throw new Error("Server error: " + JSON.stringify(errorMessage))
  }

  return responseData as T
}
```