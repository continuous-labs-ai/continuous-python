# ListSimulationAdvanceEventsRequest


## Fields

| Field                                                              | Type                                                               | Required                                                           | Description                                                        |
| ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `id`                                                               | *str*                                                              | :heavy_check_mark:                                                 | Simulation ID.                                                     |
| `advance_id`                                                       | *str*                                                              | :heavy_check_mark:                                                 | Advance ID. A historical fork can read inherited runtime receipts. |
| `limit`                                                            | *Optional[int]*                                                    | :heavy_minus_sign:                                                 | Page size. Values below 1 use 50. Values above 200 use 200.        |
| `cursor`                                                           | *Optional[str]*                                                    | :heavy_minus_sign:                                                 | Opaque next_cursor value from a previous page.                     |