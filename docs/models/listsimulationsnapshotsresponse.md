# ListSimulationSnapshotsResponse


## Fields

| Field                                                              | Type                                                               | Required                                                           | Description                                                        |
| ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `next_cursor`                                                      | *Nullable[str]*                                                    | :heavy_check_mark:                                                 | Cursor for the next page, or null.                                 |
| `simulation_snapshots`                                             | List[[models.SimulationSnapshot](../models/simulationsnapshot.md)] | :heavy_check_mark:                                                 | Snapshots in this page.                                            |