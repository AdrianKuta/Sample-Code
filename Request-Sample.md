# Request Sample

```Typescript
import type { DraftProfile } from "@/types"

import { API_BASE_URL, TYK_GATE_URL, USES_MOCK } from "@/lib/env"
import { toggleServerSideOptions } from "@/lib/utils"

import { baseFetch } from "./base-fetch"
import { type Cost } from "./target-groups"

export type GetFeasibilityResponseObject = {
  current: number
  goal: number
}

export type GetTargetGroupFeasibilityPayload = {
  collects_pii?: boolean
  filling_goal?: number
  incidence_rate: number
  length_of_interview: number
  locale: string
  price?: Cost
  start_date?: Date
  end_date?: Date
  profiles?: DraftProfile[]
  name?: string
}

export type GetTargetGroupFeasibilityProps = {
  accountId: string
  projectId: string
  payload: GetTargetGroupFeasibilityPayload
}

export type GetExistingTargetGroupFeasibilityProps = Pick<
  GetTargetGroupFeasibilityProps,
  "accountId" | "projectId"
> & {
  targetGroupId: string
  payload?: Partial<GetTargetGroupFeasibilityPayload>
}

type Completes = {
  nominal: number
  percentage: number
}

export type MinMaxCompletes = {
  min_completes: Completes
  max_completes: Completes
}

type Quota = {
  min_completes: Completes
  max_completes: Completes
}

export type QuotaFeasibility = {
  ungrouped_quotas: MinMaxCompletes[]
  grouped_quotas: MinMaxCompletes[]
}

type Quotas = {
  ungrouped: Quota[]
  grouped: Quota[]
}

export type FeasibilityProfile = {
  question_id: number
  quotas: Quotas
}

export type FeasibilityProfiles = FeasibilityProfile[]

export type GetTargetGroupFeasibilityResponseObject = {
  profiles: FeasibilityProfiles
  suggested_price: Cost
  suggested_price_range?: { min: Cost; max: Cost }
  suggested_filling_goal_range?: { min: number; max: number }
}

export type GetExistingTargetGroupFeasibilityResponseObject = {
  suggested_price: Cost
  suggested_price_range: { min: Cost; max: Cost }
  suggested_filling_goal_range: { min: number; max: number }
  per_quota_feasibility: QuotaFeasibility[]
}

export async function getTargetGroupFeasibility({
  accountId,
  projectId,
  payload,
}: GetTargetGroupFeasibilityProps) {
  return baseFetch<GetTargetGroupFeasibilityResponseObject>(
    `${API_BASE_URL}/api/accounts/${accountId}/projects/${projectId}/target-groups/feasibility`,
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(payload),
    }
  )
}

export async function getExistingTargetGroupFeasibility({
  accountId,
  projectId,
  targetGroupId,
  payload,
}: GetExistingTargetGroupFeasibilityProps) {
  const baseUrl = toggleServerSideOptions(
    USES_MOCK ? `${API_BASE_URL}/api` : `${TYK_GATE_URL}/demand`,
    "/api"
  )
  return baseFetch<GetExistingTargetGroupFeasibilityResponseObject>(
    `${baseUrl}/accounts/${accountId}/projects/${projectId}/target-groups/${targetGroupId}/feasibility`,
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(payload),
    }
  )
}
```