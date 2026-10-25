# CreateSimulationSnapshotRequest


## Fields

| Field                                                                    | Type                                                                     | Required                                                                 | Description                                                              |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `at_step`                                                                | *Optional[int]*                                                          | :heavy_minus_sign:                                                       | Completed step to save the state after. Omission saves the latest state. |
| `name`                                                                   | *str*                                                                    | :heavy_check_mark:                                                       | Snapshot name.                                                           |
| `simulation_id`                                                          | *str*                                                                    | :heavy_check_mark:                                                       | ID of the running Simulation to save.                                    |