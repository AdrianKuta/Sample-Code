# Typography Story Book

```Typescript
import type { Meta, StoryObj } from "@storybook/react"

import { Typography } from "./typography"

const meta: Meta<typeof Typography> = {
  title: "Foundation/Typography",
  component: Typography,
  tags: ["autodocs"],
  parameters: {},
}

export default meta
type Story = StoryObj<typeof Typography>

export const Heading: Story = {
  render: () => (
    <>
      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography tag="title1" className="heading-6xl">
          heading-6xl
        </Typography>
        <Typography tag="title2" className="heading-5xl">
          heading-5xl
        </Typography>
        <Typography tag="title3" className="heading-4xl">
          heading-4xl
        </Typography>
        <Typography tag="title4" className="heading-3xl">
          heading-3xl
        </Typography>
        <Typography tag="title5" className="heading-2xl">
          heading-2xl
        </Typography>
        <Typography tag="title6" className="heading-xl">
          heading-xl
        </Typography>
        <Typography tag="title6" className="heading-lg">
          heading-lg
        </Typography>
      </div>

      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography tag="title1" className="heading-6xl-semibold">
          heading-6xl-semibold
        </Typography>
        <Typography tag="title2" className="heading-5xl-semibold">
          heading-5xl-semibold
        </Typography>
        <Typography tag="title3" className="heading-4xl-semibold">
          heading-4xl-semibold
        </Typography>
        <Typography tag="title4" className="heading-3xl-semibold">
          heading-3xl-semibold
        </Typography>
        <Typography tag="title5" className="heading-2xl-semibold">
          heading-2xl-semibold
        </Typography>
        <Typography tag="title6" className="heading-xl-semibold">
          heading-xl-semibold
        </Typography>
        <Typography tag="title6" className="heading-lg-semibold">
          heading-lg-semibold
        </Typography>
      </div>

      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography tag="title1" className="heading-6xl-bold">
          heading-6xl-bold
        </Typography>
        <Typography tag="title2" className="heading-5xl-bold">
          heading-5xl-bold
        </Typography>
        <Typography tag="title3" className="heading-4xl-bold">
          heading-4xl-bold
        </Typography>
        <Typography tag="title4" className="heading-3xl-bold">
          heading-3xl-bold
        </Typography>
        <Typography tag="title5" className="heading-2xl-bold">
          heading-2xl-bold
        </Typography>
        <Typography tag="title6" className="heading-xl-bold">
          heading-xl-bold
        </Typography>
        <Typography tag="title6" className="heading-lg-bold">
          heading-lg-bold
        </Typography>
        <Typography tag="title6" className="heading-base-bold">
          heading-base-bold
        </Typography>
        <Typography tag="title6" className="heading-sm-bold">
          heading-sm-bold
        </Typography>
        <Typography tag="title6" className="heading-xs-bold-caps">
          heading-xs-bold-caps
        </Typography>
      </div>
    </>
  ),
}

export const Data: Story = {
  render: () => (
    <>
      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography tag="title1" className="data-6xl-extrabold">
          data-6xl-extrabold
        </Typography>
        <Typography tag="title2" className="data-5xl-extrabold">
          data-5xl-extrabold
        </Typography>
        <Typography tag="title3" className="data-4xl-extrabold">
          data-4xl-extrabold
        </Typography>
        <Typography tag="title4" className="data-3xl-extrabold">
          data-3xl-extrabold
        </Typography>
        <Typography tag="title5" className="data-2xl-extrabold">
          data-2xl-extrabold
        </Typography>
        <Typography tag="title6" className="data-xl-extrabold">
          data-xl-extrabold
        </Typography>
        <Typography tag="title6" className="data-lg-extrabold">
          data-lg-extrabold
        </Typography>
        <Typography tag="title6" className="data-base-extrabold">
          data-base-extrabold
        </Typography>
        <Typography tag="title6" className="data-sm-extrabold">
          data-sm-extrabold
        </Typography>
        <Typography tag="title6" className="data-xs-extrabold">
          data-xs-extrabold
        </Typography>
      </div>
    </>
  ),
}

export const Body: Story = {
  render: () => (
    <>
      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography className="text-base">text-base</Typography>
        <Typography className="text-base-link">text-base-link</Typography>
        <Typography className="text-sm">text-sm</Typography>
        <Typography className="text-sm-link">text-sm-link</Typography>
        <Typography className="text-xs">text-xs</Typography>
        <Typography className="text-xs-link">text-xs-link</Typography>
      </div>

      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography className="text-base-medium">text-base-medium</Typography>
        <Typography className="text-base-medium-link">
          text-base-medium-link
        </Typography>
        <Typography className="text-sm-medium">text-sm-medium</Typography>
        <Typography className="text-sm-medium-link">
          text-sm-medium-link
        </Typography>
        <Typography className="text-xs-medium">text-xs-medium</Typography>
        <Typography className="text-xs-medium-link">
          text-xs-medium-link
        </Typography>
      </div>

      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography className="text-base-bold">text-base-bold</Typography>
        <Typography className="text-base-bold-link">
          text-base-bold-link
        </Typography>
        <Typography className="text-sm-bold">text-sm-bold</Typography>
        <Typography className="text-sm-bold-link">text-sm-bold-link</Typography>
        <Typography className="text-xs-bold">text-xs-bold</Typography>
        <Typography className="text-xs-bold-link">text-xs-bold-link</Typography>
      </div>
    </>
  ),
}

export const Label: Story = {
  render: () => (
    <>
      <div className="my-6 flex flex-col gap-2 text-primary">
        <Typography tag="label" className="label-xs-semibold-caps">
          label-xs-semibold-caps
        </Typography>
        <Typography tag="label" className="label-xs-semibold">
          label-xs-semibold
        </Typography>
        <Typography tag="label" className="label-sm-semibold">
          label-sm-semibold
        </Typography>
        <Typography tag="label" className="label-base-semibold">
          label-base-semibold
        </Typography>
      </div>
    </>
  ),
}
```