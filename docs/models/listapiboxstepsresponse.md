# ListAPIBoxStepsResponse


## Fields

| Field                                              | Type                                               | Required                                           | Description                                        |
| -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| `next_cursor`                                      | *Nullable[str]*                                    | :heavy_check_mark:                                 | Cursor for the next page, or null.                 |
| `steps`                                            | List[[models.ApiboxStep](../models/apiboxstep.md)] | :heavy_check_mark:                                 | Steps in this page.                                |