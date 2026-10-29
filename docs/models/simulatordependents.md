# SimulatorDependents


## Fields

| Field                                                                  | Type                                                                   | Required                                                               | Description                                                            |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `child_simulators`                                                     | List[*str*]                                                            | :heavy_check_mark:                                                     | IDs of Simulators built from the Simulator.                            |
| `simulations`                                                          | List[*str*]                                                            | :heavy_check_mark:                                                     | IDs of Simulations created from the Simulator.                         |
| `snapshots`                                                            | List[*str*]                                                            | :heavy_check_mark:                                                     | IDs of Simulation Snapshots of Simulations created from the Simulator. |