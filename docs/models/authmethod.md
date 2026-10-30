# AuthMethod

How the client authenticates: client_secret, or private_key_jwt, a JWT assertion signed with its private key.

## Example Usage

```python
from continuous.models import AuthMethod

# Open enum: unrecognized values are captured as UnrecognizedStr
value: AuthMethod = "client_secret"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"client_secret"`
- `"private_key_jwt"`
