# CloneSimulatorRequest


## Fields

| Field                                                                        | Type                                                                         | Required                                                                     | Description                                                                  |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `idempotency_key`                                                            | *str*                                                                        | :heavy_check_mark:                                                           | Stable key for retrying this clone into the target workspace.                |
| `name`                                                                       | *Optional[str]*                                                              | :heavy_minus_sign:                                                           | Display name for the clone. Defaults to the source Simulator's name.         |
| `target_workspace_id`                                                        | *str*                                                                        | :heavy_check_mark:                                                           | Workspace that receives the clone. It must differ from the source workspace. |