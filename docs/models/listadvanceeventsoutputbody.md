# ListAdvanceEventsOutputBody


## Fields

| Field                                              | Type                                               | Required                                           | Description                                        |
| -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| `events`                                           | List[[models.ClockEvent](../models/clockevent.md)] | :heavy_check_mark:                                 | Committed events in execution order.               |
| `next_cursor`                                      | *Nullable[str]*                                    | :heavy_check_mark:                                 | Cursor for the next page, or null.                 |