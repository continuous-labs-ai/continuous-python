# ListMCPBoxSnapshotsResponse


## Fields

| Field                                                      | Type                                                       | Required                                                   | Description                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `next_cursor`                                              | *Nullable[str]*                                            | :heavy_check_mark:                                         | Cursor for the next page, or null.                         |
| `snapshots`                                                | List[[models.MCPBoxSnapshot](../models/mcpboxsnapshot.md)] | :heavy_check_mark:                                         | MCP Box Snapshots in this page.                            |