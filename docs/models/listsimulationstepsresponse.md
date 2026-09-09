# ListSimulationStepsResponse


## Fields

| Field                                  | Type                                   | Required                               | Description                            |
| -------------------------------------- | -------------------------------------- | -------------------------------------- | -------------------------------------- |
| `next_cursor`                          | *Nullable[str]*                        | :heavy_check_mark:                     | Cursor for the next page, or null.     |
| `steps`                                | List[[models.Step](../models/step.md)] | :heavy_check_mark:                     | Recorded steps in this page.           |