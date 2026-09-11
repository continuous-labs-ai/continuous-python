# WorldRecordCount


## Fields

| Field                                                      | Type                                                       | Required                                                   | Description                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `count`                                                    | *int*                                                      | :heavy_check_mark:                                         | Number of stored starting records of this type.            |
| `entity`                                                   | *str*                                                      | :heavy_check_mark:                                         | Record type reported by the Simulator.                     |
| `member_index`                                             | *int*                                                      | :heavy_check_mark:                                         | Position in the selected Simulator list, starting at zero. |
| `simulator_id`                                             | *str*                                                      | :heavy_check_mark:                                         | Stable ID of the Simulator for this member.                |