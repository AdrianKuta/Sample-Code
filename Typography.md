# Typography

```Typescript
import React from "react"
import type { VariantProps } from "class-variance-authority"
import { cva } from "class-variance-authority"

import { cn } from "@/lib/utils"

const defaultVariantMapping = {
  title1: "h1",
  title2: "h2",
  title3: "h3",
  title4: "h4",
  title5: "h5",
  title6: "h6",
  subtitle1: "h6",
  subtitle2: "h6",
  body1: "p",
  body2: "p",
  inherit: "p",
  label: "label",
}

const typographyVariant = cva("", {
  variants: {
    variant: {
      default: "text-base",
      heading: "heading",
      subtitle: "text-sm text-secondary",
      label: "label",
    },
  },
  defaultVariants: {
    variant: "default",
  },
})

export type FontWeight = "normal" | "medium" | "semibold" | "bold" | "extrabold"

export interface TypographyProps
  extends React.HTMLAttributes<HTMLBaseElement>,
    VariantProps<typeof typographyVariant> {
  tag?: keyof typeof defaultVariantMapping
  fontWeight?: FontWeight
}

const Typography = React.forwardRef<HTMLElement, TypographyProps>(
  function Typography(
    {
      className,
      tag = "body1",
      children,
      variant,
      fontWeight = "normal",
      ...props
    },
    ref
  ) {
    const fontWeightClass = {
      normal: "font-normal",
      medium: "font-medium",
      semibold: "font-semibold",
      bold: "font-bold",
      extrabold: "font-extrabold",
    }[fontWeight]

    const isHeadingOrSubtitle = [
      "title1",
      "title2",
      "title3",
      "title4",
      "title5",
      "title6",
      "subtitle1",
      "subtitle2",
    ].includes(tag)

    return React.createElement(
      defaultVariantMapping[tag],
      {
        className: cn(
          isHeadingOrSubtitle
            ? typographyVariant({ variant: "heading" })
            : typographyVariant({ variant }),
          fontWeightClass,
          className
        ),
        ref,
        ...props,
      },
      children
    )
  }
)

export { Typography }
```