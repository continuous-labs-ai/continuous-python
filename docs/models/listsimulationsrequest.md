# ListSimulationsRequest


## Fields

| Field                                                                        | Type                                                                         | Required                                                                     | Description                                                                  |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `status`                                                                     | [Optional[models.ListSimulationsStatus]](../models/listsimulationsstatus.md) | :heavy_minus_sign:                                                           | Optional lifecycle status filter.                                            |
| `simulator_id`                                                               | *Optional[str]*                                                              | :heavy_minus_sign:                                                           | Optional stable Simulator ID filter.                                         |
| `limit`                                                                      | *Optional[int]*                                                              | :heavy_minus_sign:                                                           | Page size. Values below 1 use 50. Values above 200 use 200.                  |
| `cursor`                                                                     | *Optional[str]*                                                              | :heavy_minus_sign:                                                           | Opaque next_cursor value from a previous page.                               |