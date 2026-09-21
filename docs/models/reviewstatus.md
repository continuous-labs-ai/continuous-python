# ReviewStatus

Status of the original independent review, not approval of later edits. Null before review.

## Example Usage

```python
from continuous.models import ReviewStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: ReviewStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"accepted"`
- `"rejected"`
