# World


## Fields

| Field                                                                | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `created_at`                                                         | [date](https://docs.python.org/3/library/datetime.html#date-objects) | :heavy_check_mark:                                                   | Time when the World build started.                                   |
| `error`                                                              | [Nullable[models.ResourceError]](../models/resourceerror.md)         | :heavy_check_mark:                                                   | N/A                                                                  |
| `id`                                                                 | *str*                                                                | :heavy_check_mark:                                                   | Stable World ID.                                                     |
| `instructions`                                                       | *str*                                                                | :heavy_check_mark:                                                   | Build guidance stored with the World.                                |
| `simulations`                                                        | List[[models.WorldSimulation](../models/worldsimulation.md)]         | :heavy_check_mark:                                                   | Created member Simulations. This list is empty before first start.   |
| `simulators`                                                         | List[*str*]                                                          | :heavy_check_mark:                                                   | Stable Simulator IDs in member order.                                |
| `status`                                                             | [models.WorldStatus](../models/worldstatus.md)                       | :heavy_check_mark:                                                   | Current World lifecycle status.                                      |