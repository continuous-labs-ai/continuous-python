<!-- Start SDK Example Usage [usage] -->
```python
# Synchronous Example
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()
```

</br>

The same SDK client can also be used to make asynchronous requests by importing asyncio.

```python
# Asynchronous Example
import asyncio
from continuous import Continuous
import os

async def main():

    async with Continuous(
        api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
    ) as c_client:

        res = await c_client.apibox.list_apiboxes_async(limit=50)

        while res is not None:
            # Handle items

            res = res.next()

asyncio.run(main())
```
<!-- End SDK Example Usage [usage] -->