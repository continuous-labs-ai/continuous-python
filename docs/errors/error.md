# Error


## Fields

| Field                                                                            | Type                                                                             | Required                                                                         | Description                                                                      |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `code`                                                                           | *str*                                                                            | :heavy_check_mark:                                                               | Stable machine-readable error code.                                              |
| `detail`                                                                         | *str*                                                                            | :heavy_check_mark:                                                               | Safe human-readable error detail.                                                |
| `validation`                                                                     | [Nullable[models.SpecificationValidation]](../models/specificationvalidation.md) | :heavy_check_mark:                                                               | N/A                                                                              |