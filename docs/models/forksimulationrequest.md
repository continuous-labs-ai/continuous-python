# ForkSimulationRequest


## Fields

| Field                                                              | Type                                                               | Required                                                           | Description                                                        |
| ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `at_step`                                                          | *Optional[int]*                                                    | :heavy_minus_sign:                                                 | Completed step to fork from. Omission forks from the latest state. |
| `name`                                                             | *Optional[str]*                                                    | :heavy_minus_sign:                                                 | Optional child Simulation name. Omission generates a name.         |