# Sample Hooks

## useGetProjects

```Typescript
import { useCallback, useState } from "react"

import type { BaseError } from "@/api/base-fetch"
import {
  getProjects,
  type GetProjectsParams,
  type GetProjectsResponse,
} from "@/api/projects"
import logger from "@/lib/logger"

export const useGetProjects = (
  accountId: string
): {
  refetch: (
    params: GetProjectsParams
  ) => Promise<GetProjectsResponse | undefined>
  isLoading: boolean
} => {
  const [isLoading, setIsLoading] = useState<boolean>(false)

  const fetchData = useCallback(
    async (params: GetProjectsParams) => {
      try {
        setIsLoading(true)
        const response = (await getProjects(
          accountId,
          params
        )) as GetProjectsResponse

        return response
      } catch (error) {
        logger.error("Failed to fetch projects", error as BaseError)
      } finally {
        setIsLoading(false)
      }
    },
    [accountId]
  )

  const refetch = (params: GetProjectsParams) => fetchData(params)

  return { refetch, isLoading }
}
```

## useGetTargetGroups

```Typescript
import { useCallback, useState } from "react"

import {
  getTargetGroups,
  type GetTargetGroupsListResponse,
  type TargetGroupParams,
} from "@/api/target-groups"
import logger from "@/lib/logger"
import { isHandledErrorResponse } from "@/app/api/_utils/is-handled-error-response"

export const useGetTargetGroups = (
  accountId: string,
  projectId: string
): {
  refetch: (
    params: TargetGroupParams
  ) => Promise<GetTargetGroupsListResponse | undefined>
  isLoading: boolean
  failed: boolean
} => {
  const [isLoading, setIsLoading] = useState<boolean>(false)
  const [failed, setFailed] = useState<boolean>(false)

  const fetchData = useCallback(
    async (params: TargetGroupParams) => {
      try {
        setIsLoading(true)
        const response = await getTargetGroups(accountId, projectId, params)
        setIsLoading(false)

        if (isHandledErrorResponse(response)) {
          setFailed(true)
          logger.error("Received handled error response")
          return
        }

        setFailed(false)
        return response as GetTargetGroupsListResponse
      } catch (error) {
        setFailed(true)
        logger.error("Failed to fetch target-groups")
      }
    },
    [accountId, projectId]
  )

  const refetch = useCallback(
    (params: TargetGroupParams) => fetchData(params),
    [fetchData]
  )

  return { refetch, isLoading, failed }
}
```