# BuildWorldRequest


## Fields

| Field                                                                                      | Type                                                                                       | Required                                                                                   | Description                                                                                |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `instructions`                                                                             | *Optional[str]*                                                                            | :heavy_minus_sign:                                                                         | Build guidance. At most 16,384 characters and 65,536 UTF-8 bytes. U+0000 is not permitted. |
| `simulators`                                                                               | List[*str*]                                                                                | :heavy_check_mark:                                                                         | Stable Simulator IDs for the World.                                                        |