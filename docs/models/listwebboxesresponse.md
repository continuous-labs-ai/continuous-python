# ListWebBoxesResponse


## Fields

| Field                                      | Type                                       | Required                                   | Description                                |
| ------------------------------------------ | ------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| `next_cursor`                              | *Nullable[str]*                            | :heavy_check_mark:                         | Cursor for the next page, or null.         |
| `webboxes`                                 | List[[models.WebBox](../models/webbox.md)] | :heavy_check_mark:                         | Web Boxes in this page.                    |