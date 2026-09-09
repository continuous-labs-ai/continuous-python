# Simulator


## Fields

| Field                                                                | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `created_at`                                                         | [date](https://docs.python.org/3/library/datetime.html#date-objects) | :heavy_check_mark:                                                   | Simulator creation time.                                             |
| `error`                                                              | [Nullable[models.ResourceError]](../models/resourceerror.md)         | :heavy_check_mark:                                                   | N/A                                                                  |
| `id`                                                                 | *str*                                                                | :heavy_check_mark:                                                   | Stable Simulator ID.                                                 |
| `name`                                                               | *str*                                                                | :heavy_check_mark:                                                   | Simulator name. Names cannot start with smr_.                        |
| `parent_id`                                                          | *Nullable[str]*                                                      | :heavy_check_mark:                                                   | Stable parent Simulator ID, or null.                                 |
| `source`                                                             | [models.Source](../models/source.md)                                 | :heavy_check_mark:                                                   | Simulator source.                                                    |
| `status`                                                             | [models.SimulatorStatus](../models/simulatorstatus.md)               | :heavy_check_mark:                                                   | Current build status.                                                |