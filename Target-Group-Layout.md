# Target Group Layout

```Typescript
import { Suspense } from "react"
import { redirect } from "next/navigation"

import { getManagedProfiles } from "@/api/profiling"
import type { TargetGroupDetailsResponse } from "@/api/target-groups"
import {
  getTargetGroupDetails,
  getTargetGroupOverview,
  TargetGroupStatus,
  type GetTargetGroupOverviewResponseObject,
} from "@/api/target-groups"
import { getServerSession } from "@/lib/auth"
import { isSuperAdmin } from "@/lib/utils"
import { GenericLayoutWithHeader } from "@/components/layout/generic-layout-with-header"

import { TargetGroupOverviewHeader } from "./_components/target-group-overview-header"
import { TargetGroupStatistics } from "./_components/target-group-statistics"
import { TargetGroupStatisticsSkeleton } from "./_components/target-group-statistics-skeleton"
import { TargetGroupTabs } from "./_components/target-group-tabs"
import { TargetGroupOverviewHeaderSkeleton } from "./_components/target-groups-overview-header-skeleton"
import { TargetGroupProvider } from "./_context/target-group-provider"
import { ManageProfilesProvider } from "./profiling/_contexts/manage-profiles-context"

export default async function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: { accountId: string; projectId: string; targetGroupId: string }
}) {
  const session = await getServerSession()
  const userAccountId = session?.user?.defaultAccountId
  const { accountId: accountIdStringParam, projectId, targetGroupId } = params
  const accountIdParam = parseInt(accountIdStringParam, 10)
  const accountId = isNaN(accountIdParam)
    ? userAccountId
    : accountIdParam || userAccountId

  const overview = (await getTargetGroupOverview(
    projectId,
    targetGroupId,
    Number(accountId)!
  )) as GetTargetGroupOverviewResponseObject

  const details = (await getTargetGroupDetails(
    accountId as number,
    projectId,
    targetGroupId
  )) as TargetGroupDetailsResponse

  const onGetManagedProfiles = async () => {
    return getManagedProfiles({
      accountId: accountId as number,
      projectId,
      targetGroupId,
    })
  }

  const profiles = await onGetManagedProfiles()

  if (overview?.status === TargetGroupStatus.DRAFT)
    redirect(`/projects/setup?target_group_id=${targetGroupId}`)

  return (
    <TargetGroupProvider
      initialValue={{
        accountId: accountId,
        projectId: projectId,
        projectName: overview.project_name,
        currentCostPerInterview: overview?.current_cost_per_interview,
        averageCostPerInterview: overview?.average_cost_per_interview,
        currentCost: overview?.current_cost,
        currentCompletes: overview?.progress?.current_completes,
        currentPrescreens: overview?.progress?.current_prescreens,
        targetGroupName: overview?.target_group_name,
        targetGroupId: targetGroupId,
        targetGroupStatus: overview?.status,
        fillingStrategy: overview?.progress?.filling_strategy,
        fillingGoal: overview?.progress?.filling_goal,
        country: overview?.country_code,
        conversionRate: overview?.conversion_rate,
        dropOffRate: overview?.drop_off_rate,
        incidenceRate: overview?.incidence_rate,
        lengthOfInterview: overview?.median_length_of_interview_seconds,
        pricingModel: overview?.pricing_model,
        isPricingRestricted: overview?.is_pricing_restricted,
        unsavedChanges: undefined,
        locale: details?.locale,
        profiles,
      }}
    >
      <ManageProfilesProvider
        accountId={accountId ?? 0}
        projectId={projectId}
        targetGroupId={targetGroupId}
      >
        <GenericLayoutWithHeader
          header={
            <Suspense fallback={<TargetGroupOverviewHeaderSkeleton />}>
              <TargetGroupOverviewHeader
                accountId={accountId?.toString()}
                isSuperAdmin={isSuperAdmin(session)}
              />
            </Suspense>
          }
        >
          <>
            <Suspense fallback={<TargetGroupStatisticsSkeleton />}>
              <TargetGroupStatistics />
            </Suspense>

            <TargetGroupTabs>
              <main>{children}</main>
            </TargetGroupTabs>
          </>
        </GenericLayoutWithHeader>
      </ManageProfilesProvider>
    </TargetGroupProvider>
  )
}
```