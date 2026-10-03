# SpecificationWarning


## Fields

| Field                                                                    | Type                                                                     | Required                                                                 | Description                                                              |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `code`                                                                   | [models.SpecificationWarningCode](../models/specificationwarningcode.md) | :heavy_check_mark:                                                       | Stable warning code.                                                     |
| `location`                                                               | *str*                                                                    | :heavy_check_mark:                                                       | Readable endpoint or field affected by this warning.                     |
| `message`                                                                | *str*                                                                    | :heavy_check_mark:                                                       | What the specification declares and how the build handles it.            |
| `operations`                                                             | List[*str*]                                                              | :heavy_check_mark:                                                       | Always empty.                                                            |