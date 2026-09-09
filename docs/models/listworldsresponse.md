# ListWorldsResponse


## Fields

| Field                                                         | Type                                                          | Required                                                      | Description                                                   |
| ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| `next_cursor`                                                 | *Nullable[str]*                                               | :heavy_check_mark:                                            | Cursor for the next page, or null when this is the last page. |
| `worlds`                                                      | List[[models.World](../models/world.md)]                      | :heavy_check_mark:                                            | Worlds in this page.                                          |