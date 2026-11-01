# Apibox

## Overview

Start API Boxes from API Box Snapshots. Each API Box serves its Snapshot's API at its own endpoint, with its own state, in real time. Stop, start and delete them.

### Available Operations

* [list_apiboxes](#list_apiboxes) - List API Boxes
* [create_apibox](#create_apibox) - Create API Box
* [delete_apibox](#delete_apibox) - Delete API Box
* [get_apibox](#get_apibox) - Get API Box
* [update_apibox](#update_apibox) - Update API Box
* [~~get_apibox_connection~~](#get_apibox_connection) - Get API Box connection :warning: **Deprecated**
* [save_apibox_snapshot](#save_apibox_snapshot) - Save API Box as Snapshot
* [start_apibox](#start_apibox) - Start API Box
* [list_apibox_steps](#list_apibox_steps) - List API Box steps
* [stop_apibox](#stop_apibox) - Stop API Box

## list_apiboxes

Returns the workspace's API Boxes, newest first. Results can be filtered by status and Snapshot.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-apiboxes" method="get" path="/v1/apibox" -->
```python
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

### Parameters

| Parameter                                                                 | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `status`                                                                  | [Optional[models.ListApiboxesStatus]](../../models/listapiboxesstatus.md) | :heavy_minus_sign:                                                        | Only API Boxes with this status.                                          |
| `snapshot_id`                                                             | *Optional[str]*                                                           | :heavy_minus_sign:                                                        | Only API Boxes started from this Snapshot.                                |
| `limit`                                                                   | *Optional[int]*                                                           | :heavy_minus_sign:                                                        | Page size. Values below 1 use 50. Values above 200 use 200.               |
| `cursor`                                                                  | *Optional[str]*                                                           | :heavy_minus_sign:                                                        | Opaque next_cursor value from a previous page.                            |
| `retries`                                                                 | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)          | :heavy_minus_sign:                                                        | Configuration to override the default retry behavior of the client.       |

### Response

**[models.ListApiboxesResponseResponse](../../models/listapiboxesresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_apibox

Creates an API Box from a ready API Box Snapshot and starts it. It returns at once with status starting and the API Box's token; get the API Box until it is running. Send the token in X-Continuous-Token on each request to the API Box's endpoint. The token lasts as long as the API Box, and its get returns it again. Each API Box has its own state, and runs in real time.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-apibox" method="post" path="/v1/apibox" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.create_apibox(snapshot_id="abs_01J8Z5X4K70000000000000002", metadata={
        "customer_id": "cust_123",
    }, name="billing-sandbox")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                   | Type                                                                                                                                                                                                                        | Required                                                                                                                                                                                                                    | Description                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `snapshot_id`                                                                                                                                                                                                               | *str*                                                                                                                                                                                                                       | :heavy_check_mark:                                                                                                                                                                                                          | A ready API Box Snapshot of the workspace or the catalog.                                                                                                                                                                   |
| `idempotency_key`                                                                                                                                                                                                           | *Optional[str]*                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                          | Optional key for retrying this create safely. A retry with the same key and request returns the same API Box and token. The same key with a different request returns 409 idempotency_conflict.                             |
| `include_sample_data`                                                                                                                                                                                                       | *Optional[bool]*                                                                                                                                                                                                            | :heavy_minus_sign:                                                                                                                                                                                                          | Start with the Snapshot's sample data: a realistic account loaded on top of its seed before the first request. Defaults to false, a brand-new workspace. Requires a Snapshot that carries sample data; see its composition. |
| `metadata`                                                                                                                                                                                                                  | *Optional[Any]*                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                          | Customer JSON metadata, up to 16 KiB and 64 nesting levels.                                                                                                                                                                 |
| `name`                                                                                                                                                                                                                      | *Optional[str]*                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                          | Name for the API Box. Omission generates a name.                                                                                                                                                                            |
| `retries`                                                                                                                                                                                                                   | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                            | :heavy_minus_sign:                                                                                                                                                                                                          | Configuration to override the default retry behavior of the client.                                                                                                                                                         |

### Response

**[models.APIBoxTokenResponse](../../models/apiboxtokenresponse.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## delete_apibox

Deletes an API Box, running or stopped, with its state. Its runtime is released after the response. An API Box that MCP Boxes front returns 409 apibox_in_use; delete them first.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-apibox" method="delete" path="/v1/apibox/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.apibox.delete_apibox(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_apibox

Returns an API Box, its status and its connection: its endpoint and token, whether it is running or stopped. Requests reach it only while it is running.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-apibox" method="get" path="/v1/apibox/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.get_apibox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.GetAPIBoxResponse](../../models/getapiboxresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_apibox

Renames an API Box or replaces its metadata. Omitted fields keep their current values.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-apibox" method="patch" path="/v1/apibox/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.update_apibox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | API Box ID.                                                                                  |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New name.                                                                                    |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.APIBox](../../models/apibox.md)**

### Errors

| Error Type                        | Status Code                       | Content Type                      |
| --------------------------------- | --------------------------------- | --------------------------------- |
| errors.Error                      | 400, 401, 404, 408, 413, 415, 422 | application/problem+json          |
| errors.Error                      | 500, 503                          | application/problem+json          |
| errors.ContinuousDefaultError     | 4XX, 5XX                          | \*/\*                             |

## ~~get_apibox_connection~~

Deprecated: get the API Box, whose connection this returns. Returns the API Box's endpoint and token, whether it is running or stopped. An API Box with no stored token returns 409 apibox_token_unavailable; its next start issues one.

> :warning: **DEPRECATED**: This will be removed in a future release, please migrate away from it as soon as possible.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-apibox-connection" method="get" path="/v1/apibox/{id}/connection" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.get_apibox_connection(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.APIBoxConnection](../../models/apiboxconnection.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## save_apibox_snapshot

Saves a running API Box's service and current state as an API Box Snapshot, ready when the response returns. The API Box keeps running, and a paused one is woken. API Boxes created from the Snapshot start with the saved state instead of a new workspace, so they refuse include_sample_data. The Snapshot outlives the API Box. A starting API Box returns 409 apibox_starting; a stopping or stopped one returns 409 apibox_stopped; one whose runtime cannot save its state returns 409 apibox_rebuild_required.

### Example Usage

<!-- UsageSnippet language="python" operationID="save-apibox-snapshot" method="post" path="/v1/apibox/{id}/snapshot" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.save_apibox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                              | Type                                                                                                                                                                                                   | Required                                                                                                                                                                                               | Description                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `id`                                                                                                                                                                                                   | *str*                                                                                                                                                                                                  | :heavy_check_mark:                                                                                                                                                                                     | ID of the Box to save.                                                                                                                                                                                 |
| `idempotency_key`                                                                                                                                                                                      | *Optional[str]*                                                                                                                                                                                        | :heavy_minus_sign:                                                                                                                                                                                     | Optional key for retrying this save safely. A retry with the same key and request returns the Snapshot the first save created. The same key with a different request returns 409 idempotency_conflict. |
| `metadata`                                                                                                                                                                                             | *Optional[Any]*                                                                                                                                                                                        | :heavy_minus_sign:                                                                                                                                                                                     | Customer JSON metadata, up to 16 KiB and 64 nesting levels.                                                                                                                                            |
| `name`                                                                                                                                                                                                 | *Optional[str]*                                                                                                                                                                                        | :heavy_minus_sign:                                                                                                                                                                                     | Name for the Snapshot. Omission generates a name.                                                                                                                                                      |
| `retries`                                                                                                                                                                                              | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                       | :heavy_minus_sign:                                                                                                                                                                                     | Configuration to override the default retry behavior of the client.                                                                                                                                    |

### Response

**[models.APIBoxSnapshot](../../models/apiboxsnapshot.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 404, 408, 409, 413, 415, 422 | application/problem+json               |
| errors.Error                           | 500, 503                               | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |

## start_apibox

Starts a stopped API Box from the state its stop saved, and returns at once with status starting; get the API Box until it is running. Its token stays the same; its get returns it. A running, starting or stopping API Box returns 409 apibox_not_stopped.

### Example Usage

<!-- UsageSnippet language="python" operationID="start-apibox" method="post" path="/v1/apibox/{id}/start" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.start_apibox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.APIBox](../../models/apibox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409, 429            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## list_apibox_steps

Lists the API Box's steps in order. Each request that changed state is one step; a request that only reads registers none. An API Box that is not running returns 409 apibox_stopped.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-apibox-steps" method="get" path="/v1/apibox/{id}/steps" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apibox_steps(id="<id>", limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box ID.                                                         |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListApiboxStepsResponse](../../models/listapiboxstepsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 404, 409, 422       | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## stop_apibox

Stops a running API Box: it saves its state, and releases its runtime; its token stays for the next start. It returns at once with status stopping; get the API Box until it is stopped. A stopped API Box is returned unchanged; a starting one returns 409 apibox_starting.

### Example Usage

<!-- UsageSnippet language="python" operationID="stop-apibox" method="post" path="/v1/apibox/{id}/stop" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.stop_apibox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.APIBox](../../models/apibox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |