<!-- Start SDK Example Usage [usage] -->
```python
# Synchronous Example
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulations(limit=50)

    # Handle response
    print(res)
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

        res = await c_client.simulations.list_simulations_async(limit=50)

        # Handle response
        print(res)

asyncio.run(main())
```
<!-- End SDK Example Usage [usage] -->