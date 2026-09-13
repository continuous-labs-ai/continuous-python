# ClockEvent


## Fields

| Field                                                                | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `at`                                                                 | [date](https://docs.python.org/3/library/datetime.html#date-objects) | :heavy_check_mark:                                                   | Simulated time observed by this event handler.                       |
| `event_id`                                                           | *str*                                                                | :heavy_check_mark:                                                   | Scheduled event ID.                                                  |
| `sequence`                                                           | *int*                                                                | :heavy_check_mark:                                                   | Persisted insertion order for events due at the same time.           |
| `type`                                                               | *str*                                                                | :heavy_check_mark:                                                   | Declared event type.                                                 |