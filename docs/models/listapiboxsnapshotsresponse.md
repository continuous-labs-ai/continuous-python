# ListAPIBoxSnapshotsResponse


## Fields

| Field                                                      | Type                                                       | Required                                                   | Description                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `next_cursor`                                              | *Nullable[str]*                                            | :heavy_check_mark:                                         | Cursor for the next page, or null.                         |
| `snapshots`                                                | List[[models.APIBoxSnapshot](../models/apiboxsnapshot.md)] | :heavy_check_mark:                                         | API Box Snapshots in this page.                            |