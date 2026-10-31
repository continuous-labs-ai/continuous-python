# ListApiboxesRequest


## Fields

| Field                                                                  | Type                                                                   | Required                                                               | Description                                                            |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `status`                                                               | [Optional[models.ListApiboxesStatus]](../models/listapiboxesstatus.md) | :heavy_minus_sign:                                                     | Only API Boxes with this status.                                       |
| `snapshot_id`                                                          | *Optional[str]*                                                        | :heavy_minus_sign:                                                     | Only API Boxes started from this Snapshot.                             |
| `limit`                                                                | *Optional[int]*                                                        | :heavy_minus_sign:                                                     | Page size. Values below 1 use 50. Values above 200 use 200.            |
| `cursor`                                                               | *Optional[str]*                                                        | :heavy_minus_sign:                                                     | Opaque next_cursor value from a previous page.                         |