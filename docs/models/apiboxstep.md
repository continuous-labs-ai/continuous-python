# ApiboxStep


## Fields

| Field                                                                | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `label`                                                              | *str*                                                                | :heavy_check_mark:                                                   | The request that made the step.                                      |
| `step`                                                               | *int*                                                                | :heavy_check_mark:                                                   | Step number, from 1 in order.                                        |
| `time`                                                               | [date](https://docs.python.org/3/library/datetime.html#date-objects) | :heavy_check_mark:                                                   | When the request ran, or null when the runtime did not record it.    |