# ListWebBoxSnapshotsResponse


## Fields

| Field                                                      | Type                                                       | Required                                                   | Description                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `next_cursor`                                              | *Nullable[str]*                                            | :heavy_check_mark:                                         | Cursor for the next page, or null.                         |
| `snapshots`                                                | List[[models.WebBoxSnapshot](../models/webboxsnapshot.md)] | :heavy_check_mark:                                         | Web Box Snapshots in this page.                            |