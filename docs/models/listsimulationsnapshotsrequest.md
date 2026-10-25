# ListSimulationSnapshotsRequest


## Fields

| Field                                                         | Type                                                          | Required                                                      | Description                                                   |
| ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| `simulation_id`                                               | *Optional[str]*                                               | :heavy_minus_sign:                                            | Optional filter for the Snapshots saved from this Simulation. |
| `limit`                                                       | *Optional[int]*                                               | :heavy_minus_sign:                                            | Page size. Values below 1 use 50. Values above 200 use 200.   |
| `cursor`                                                      | *Optional[str]*                                               | :heavy_minus_sign:                                            | Opaque next_cursor value from a previous page.                |