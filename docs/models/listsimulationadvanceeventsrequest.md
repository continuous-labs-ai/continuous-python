# ListSimulationAdvanceEventsRequest


## Fields

| Field                                                              | Type                                                               | Required                                                           | Description                                                        |
| ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `id`                                                               | *str*                                                              | :heavy_check_mark:                                                 | Simulation ID.                                                     |
| `advance_id`                                                       | *str*                                                              | :heavy_check_mark:                                                 | Advance ID. A historical fork can read inherited runtime receipts. |
| `cursor`                                                           | *Optional[str]*                                                    | :heavy_minus_sign:                                                 | Cursor from the previous page.                                     |
| `limit`                                                            | *Optional[int]*                                                    | :heavy_minus_sign:                                                 | Page size, up to 200.                                              |