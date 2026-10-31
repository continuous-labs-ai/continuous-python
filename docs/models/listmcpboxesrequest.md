# ListMcpboxesRequest


## Fields

| Field                                                                  | Type                                                                   | Required                                                               | Description                                                            |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `status`                                                               | [Optional[models.ListMcpboxesStatus]](../models/listmcpboxesstatus.md) | :heavy_minus_sign:                                                     | Only MCP Boxes with this status.                                       |
| `snapshot_id`                                                          | *Optional[str]*                                                        | :heavy_minus_sign:                                                     | Only MCP Boxes that serve this Snapshot.                               |
| `limit`                                                                | *Optional[int]*                                                        | :heavy_minus_sign:                                                     | Page size. Values below 1 use 50. Values above 200 use 200.            |
| `cursor`                                                               | *Optional[str]*                                                        | :heavy_minus_sign:                                                     | Opaque next_cursor value from a previous page.                         |