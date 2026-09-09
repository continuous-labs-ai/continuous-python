# Step


## Fields

| Field                                                                             | Type                                                                              | Required                                                                          | Description                                                                       |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `label`                                                                           | *str*                                                                             | :heavy_check_mark:                                                                | Request label. It usually contains the HTTP method and path.                      |
| `step`                                                                            | *int*                                                                             | :heavy_check_mark:                                                                | Completed request number. Use this value as at_step when you fork the Simulation. |