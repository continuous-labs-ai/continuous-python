# Simulation


## Fields

| Field                                                                | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `created_at`                                                         | [date](https://docs.python.org/3/library/datetime.html#date-objects) | :heavy_check_mark:                                                   | Simulation creation time.                                            |
| `endpoint`                                                           | *str*                                                                | :heavy_check_mark:                                                   | Data-plane endpoint for the Simulation.                              |
| `id`                                                                 | *str*                                                                | :heavy_check_mark:                                                   | Stable Simulation ID.                                                |
| `name`                                                               | *str*                                                                | :heavy_check_mark:                                                   | Simulation name.                                                     |
| `parent_id`                                                          | *Nullable[str]*                                                      | :heavy_check_mark:                                                   | Stable source Simulation ID for a fork, or null.                     |
| `simulator_id`                                                       | *str*                                                                | :heavy_check_mark:                                                   | Stable ID of the Simulator.                                          |
| `status`                                                             | [models.SimulationStatus](../models/simulationstatus.md)             | :heavy_check_mark:                                                   | Current Simulation status.                                           |