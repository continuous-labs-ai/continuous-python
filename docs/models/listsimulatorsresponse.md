# ListSimulatorsResponse


## Fields

| Field                                            | Type                                             | Required                                         | Description                                      |
| ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| `next_cursor`                                    | *Nullable[str]*                                  | :heavy_check_mark:                               | Cursor for the next page, or null.               |
| `simulators`                                     | List[[models.Simulator](../models/simulator.md)] | :heavy_check_mark:                               | Simulators in this page.                         |