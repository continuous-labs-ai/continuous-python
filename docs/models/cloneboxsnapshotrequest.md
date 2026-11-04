# CloneBoxSnapshotRequest


## Fields

| Field                                                                        | Type                                                                         | Required                                                                     | Description                                                                  |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `name`                                                                       | *Optional[str]*                                                              | :heavy_minus_sign:                                                           | Name for the clone. Defaults to the source Snapshot's name.                  |
| `target_workspace_id`                                                        | *str*                                                                        | :heavy_check_mark:                                                           | Workspace that receives the clone. It must differ from the source workspace. |