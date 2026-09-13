# AdvanceWorldTimeRequest


## Fields

| Field                                                                               | Type                                                                                | Required                                                                            | Description                                                                         |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `id`                                                                                | *str*                                                                               | :heavy_check_mark:                                                                  | Simulation or World ID.                                                             |
| `idempotency_key`                                                                   | *str*                                                                               | :heavy_check_mark:                                                                  | Stable key for this request. Reuse with the same target returns the same operation. |
| `body`                                                                              | [models.AdvanceTimeInputBody](../models/advancetimeinputbody.md)                    | :heavy_check_mark:                                                                  | N/A                                                                                 |