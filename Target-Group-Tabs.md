# Target Group Tabs

```Typescript
import { useState } from "react"
import { usePathname, useRouter } from "next/navigation"
import { useTranslations } from "next-intl"

import { TARGET_GROUP_SUBPATH } from "@/config/site"
import { splitStringAtIndex } from "@/lib/split-string-at-index"
import { Separator } from "@/ui/separator"
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/ui/tabs"

import { useTargetGroupContext } from "../_context/target-group-provider"
import { TargetGroupAction } from "../_context/types"
import { ConfirmUnsavedChangesDialog } from "./dialogs/confirm-unsaved-changes-dialog"

export function TargetGroupTabs({ children }: { children: React.ReactNode }) {
  const [openDialog, setOpenDialog] = useState<boolean>(false)
  const { isDetailsFormSaved, handleTargetGroupDispatch } =
    useTargetGroupContext()
  const [pendingTabChange, setPendingTabChange] = useState<string | null>(null)
  const router = useRouter()
  const pathName = usePathname()
  const t = useTranslations("TargetGroup")

  const [rootPath, currentPage] = splitStringAtIndex(
    pathName,
    pathName.lastIndexOf("/")
  )

  const onDialogConfirm = () => {
    setOpenDialog(false)
    if (pendingTabChange) {
      router.push(`${rootPath}${pendingTabChange}`)
      handleTargetGroupDispatch(TargetGroupAction.SET_IS_DETAILS_FORM_SAVED, {
        isDetailsFormSaved: true,
      })
      setPendingTabChange(null)
    }
  }

  const onDialogCancel = () => {
    setOpenDialog(false)
    setPendingTabChange(currentPage)
  }

  const handleTabChange = (path: string) => {
    if (!isDetailsFormSaved && currentPage === TARGET_GROUP_SUBPATH.DETAILS) {
      setPendingTabChange(path)
      setOpenDialog(true)
    } else {
      router.push(`${rootPath}${path}`)
    }
  }
  return (
    <>
      <Separator type="page" />
      <Tabs
        value={currentPage}
        onValueChange={handleTabChange}
        className="flex flex-col gap-page-sections"
      >
        <TabsList className="items-start justify-start self-stretch">
          <TabsTrigger value={TARGET_GROUP_SUBPATH.PROFILING}>
            {t("tabs.profiling")}
          </TabsTrigger>
          <TabsTrigger value={TARGET_GROUP_SUBPATH.DETAILS}>
            {t("tabs.details")}
          </TabsTrigger>
          <TabsTrigger value={TARGET_GROUP_SUBPATH.CHANGE_LOG}>
            {t("tabs.changeLog")}
          </TabsTrigger>
        </TabsList>
        <TabsContent className="mt-0" value={currentPage}>
          {children}
        </TabsContent>
      </Tabs>
      <ConfirmUnsavedChangesDialog
        isOpen={openDialog}
        onConfirm={onDialogConfirm}
        onCancel={onDialogCancel}
      />
    </>
  )
}
```