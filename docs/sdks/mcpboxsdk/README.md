# Mcpbox

## Overview

Start MCP Boxes from MCP Box Snapshots. Each MCP Box serves its Snapshot's tools over MCP in front of an API Box. Stop, start and delete them.

### Available Operations

* [list_mcpboxes](#list_mcpboxes) - List MCP Boxes
* [create_mcpbox](#create_mcpbox) - Create MCP Box
* [delete_mcpbox](#delete_mcpbox) - Delete MCP Box
* [get_mcpbox](#get_mcpbox) - Get MCP Box
* [update_mcpbox](#update_mcpbox) - Update MCP Box
* [get_mcpbox_connection](#get_mcpbox_connection) - Get MCP Box connection
* [start_mcpbox](#start_mcpbox) - Start MCP Box
* [stop_mcpbox](#stop_mcpbox) - Stop MCP Box

## list_mcpboxes

Returns the workspace's MCP Boxes, newest first. Results can be filtered by status and Snapshot.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-mcpboxes" method="get" path="/v1/mcpbox" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.list_mcpboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                                 | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `status`                                                                  | [Optional[models.ListMcpboxesStatus]](../../models/listmcpboxesstatus.md) | :heavy_minus_sign:                                                        | Only MCP Boxes with this status.                                          |
| `snapshot_id`                                                             | *Optional[str]*                                                           | :heavy_minus_sign:                                                        | Only MCP Boxes that serve this Snapshot.                                  |
| `limit`                                                                   | *Optional[int]*                                                           | :heavy_minus_sign:                                                        | Page size. Values below 1 use 50. Values above 200 use 200.               |
| `cursor`                                                                  | *Optional[str]*                                                           | :heavy_minus_sign:                                                        | Opaque next_cursor value from a previous page.                            |
| `retries`                                                                 | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)          | :heavy_minus_sign:                                                        | Configuration to override the default retry behavior of the client.       |

### Response

**[models.ListMcpboxesResponseResponse](../../models/listmcpboxesresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_mcpbox

Creates an MCP Box that serves an MCP Box Snapshot's tools over MCP in front of an API Box, and starts it. The API Box must be in the workspace and started from the API Box Snapshot the MCP Box Snapshot was built against. It returns at once with status starting and the MCP Box's token; get the MCP Box until it is running. Send MCP requests to its endpoint followed by /mcp, with the token in X-Continuous-Token. The MCP Box keeps no state: its tools read and change the API Box, and a tool call while the API Box is stopped returns the API Box's refusal as a tool error.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-mcpbox" method="post" path="/v1/mcpbox" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.create_mcpbox(apibox_id="abx_01J8Z5X4K70000000000000001", snapshot_id="mbs_01J8Z5X4K70000000000000007", name="billing-tools")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                       | Type                                                                                                                                                                                            | Required                                                                                                                                                                                        | Description                                                                                                                                                                                     |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apibox_id`                                                                                                                                                                                     | *str*                                                                                                                                                                                           | :heavy_check_mark:                                                                                                                                                                              | An API Box of the workspace, started from the API Box Snapshot the MCP Box Snapshot was built against.                                                                                          |
| `snapshot_id`                                                                                                                                                                                   | *str*                                                                                                                                                                                           | :heavy_check_mark:                                                                                                                                                                              | An MCP Box Snapshot of the workspace.                                                                                                                                                           |
| `idempotency_key`                                                                                                                                                                               | *Optional[str]*                                                                                                                                                                                 | :heavy_minus_sign:                                                                                                                                                                              | Optional key for retrying this create safely. A retry with the same key and request returns the same MCP Box and token. The same key with a different request returns 409 idempotency_conflict. |
| `metadata`                                                                                                                                                                                      | *Optional[Any]*                                                                                                                                                                                 | :heavy_minus_sign:                                                                                                                                                                              | Customer JSON metadata, up to 16 KiB and 64 nesting levels.                                                                                                                                     |
| `name`                                                                                                                                                                                          | *Optional[str]*                                                                                                                                                                                 | :heavy_minus_sign:                                                                                                                                                                              | Name for the MCP Box. Omission generates a name.                                                                                                                                                |
| `retries`                                                                                                                                                                                       | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                | :heavy_minus_sign:                                                                                                                                                                              | Configuration to override the default retry behavior of the client.                                                                                                                             |

### Response

**[models.McpboxTokenResponse](../../models/mcpboxtokenresponse.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## delete_mcpbox

Deletes an MCP Box, running or stopped. Its API Box stays. Its runtime is released after the response.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-mcpbox" method="delete" path="/v1/mcpbox/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.mcpbox.delete_mcpbox(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_mcpbox

Returns an MCP Box and its status.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-mcpbox" method="get" path="/v1/mcpbox/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.get_mcpbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.MCPBox](../../models/mcpbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_mcpbox

Renames an MCP Box or replaces its metadata. Omitted fields keep their current values.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-mcpbox" method="patch" path="/v1/mcpbox/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.update_mcpbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | MCP Box ID.                                                                                  |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New API Box name.                                                                            |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.MCPBox](../../models/mcpbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 404, 408, 413, 415  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_mcpbox_connection

Returns the MCP Box's endpoint and token, whether it is running or stopped. Requests reach it only while it is running. An MCP Box with no stored token returns 409 mcpbox_token_unavailable; its next start issues one.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-mcpbox-connection" method="get" path="/v1/mcpbox/{id}/connection" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.get_mcpbox_connection(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.McpboxConnection](../../models/mcpboxconnection.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## start_mcpbox

Starts a stopped MCP Box in front of its API Box, and returns at once with status starting and the MCP Box's token, the same one each start. A running, starting or stopping MCP Box returns 409 mcpbox_not_stopped.

### Example Usage

<!-- UsageSnippet language="python" operationID="start-mcpbox" method="post" path="/v1/mcpbox/{id}/start" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.start_mcpbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.McpboxTokenResponse](../../models/mcpboxtokenresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409, 429            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## stop_mcpbox

Stops a running MCP Box and releases its runtime; its API Box keeps running, and its token stays for the next start. It returns at once with status stopping; get the MCP Box until it is stopped. A stopped MCP Box is returned unchanged; a starting one returns 409 mcpbox_starting.

### Example Usage

<!-- UsageSnippet language="python" operationID="stop-mcpbox" method="post" path="/v1/mcpbox/{id}/stop" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox.stop_mcpbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.MCPBox](../../models/mcpbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |