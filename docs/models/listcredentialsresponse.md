# ListCredentialsResponse


## Fields

| Field                                              | Type                                               | Required                                           | Description                                        |
| -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| `credentials`                                      | List[[models.Credential](../models/credential.md)] | :heavy_check_mark:                                 | Credential metadata in this page, newest first.    |
| `next_cursor`                                      | *Nullable[str]*                                    | :heavy_check_mark:                                 | Cursor for the next page, or null.                 |