# ListSimulationsResponse


## Fields

| Field                                              | Type                                               | Required                                           | Description                                        |
| -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| `next_cursor`                                      | *Nullable[str]*                                    | :heavy_check_mark:                                 | Cursor for the next page, or null.                 |
| `simulations`                                      | List[[models.Simulation](../models/simulation.md)] | :heavy_check_mark:                                 | Simulations in this page.                          |