# Webbox

## Overview

Start Web Boxes from Web Box Snapshots. Each Web Box serves its Snapshot's web app at a host of its own, with its own state, in real time. Stop, start and delete them.

### Available Operations

* [list_webboxes](#list_webboxes) - List Web Boxes
* [create_webbox](#create_webbox) - Create Web Box
* [delete_webbox](#delete_webbox) - Delete Web Box
* [get_webbox](#get_webbox) - Get Web Box
* [update_webbox](#update_webbox) - Update Web Box
* [get_webbox_connection](#get_webbox_connection) - Get Web Box connection
* [start_webbox](#start_webbox) - Start Web Box
* [list_webbox_steps](#list_webbox_steps) - List Web Box Steps
* [stop_webbox](#stop_webbox) - Stop Web Box

## list_webboxes

Returns the workspace's Web Boxes, newest first. Results can be filtered by status and Snapshot.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-webboxes" method="get" path="/v1/webbox" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.list_webboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                                 | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `status`                                                                  | [Optional[models.ListWebboxesStatus]](../../models/listwebboxesstatus.md) | :heavy_minus_sign:                                                        | Only Web Boxes with this status.                                          |
| `snapshot_id`                                                             | *Optional[str]*                                                           | :heavy_minus_sign:                                                        | Only Web Boxes started from this Snapshot.                                |
| `limit`                                                                   | *Optional[int]*                                                           | :heavy_minus_sign:                                                        | Page size. Values below 1 use 50. Values above 200 use 200.               |
| `cursor`                                                                  | *Optional[str]*                                                           | :heavy_minus_sign:                                                        | Opaque next_cursor value from a previous page.                            |
| `retries`                                                                 | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)          | :heavy_minus_sign:                                                        | Configuration to override the default retry behavior of the client.       |

### Response

**[models.ListWebboxesResponseResponse](../../models/listwebboxesresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_webbox

Creates a Web Box from a ready Web Box Snapshot and starts it. It returns at once with status starting; get the Web Box until it is running. Open its url in a browser: the app is served at a host of its own, open to anyone with the URL. Each Web Box has its own state, and runs in real time.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-webbox" method="post" path="/v1/webbox" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.create_webbox(snapshot_id="wbs_01J8Z5X4K70000000000000005", name="crm-sandbox")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                   | Type                                                                                                                                                                                                                        | Required                                                                                                                                                                                                                    | Description                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `snapshot_id`                                                                                                                                                                                                               | *str*                                                                                                                                                                                                                       | :heavy_check_mark:                                                                                                                                                                                                          | A ready Web Box Snapshot of the workspace or the catalog.                                                                                                                                                                   |
| `idempotency_key`                                                                                                                                                                                                           | *Optional[str]*                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                          | Optional key for retrying this create safely. A retry with the same key and request returns the same Web Box. The same key with a different request returns 409 idempotency_conflict.                                       |
| `include_sample_data`                                                                                                                                                                                                       | *Optional[bool]*                                                                                                                                                                                                            | :heavy_minus_sign:                                                                                                                                                                                                          | Start with the Snapshot's sample data: a realistic account loaded on top of its seed before the first request. Defaults to false, a brand-new workspace. Requires a Snapshot that carries sample data; see its composition. |
| `metadata`                                                                                                                                                                                                                  | *Optional[Any]*                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                          | Customer JSON metadata, up to 16 KiB and 64 nesting levels.                                                                                                                                                                 |
| `name`                                                                                                                                                                                                                      | *Optional[str]*                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                          | Name for the Web Box. Omission generates a name.                                                                                                                                                                            |
| `retries`                                                                                                                                                                                                                   | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                            | :heavy_minus_sign:                                                                                                                                                                                                          | Configuration to override the default retry behavior of the client.                                                                                                                                                         |

### Response

**[models.WebBox](../../models/webbox.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 404, 408, 409, 413, 415, 429 | application/problem+json               |
| errors.Error                           | 500, 503                               | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |

## delete_webbox

Deletes a Web Box, running or stopped, with its state. Its runtime is released after the response.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-webbox" method="delete" path="/v1/webbox/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.webbox.delete_webbox(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_webbox

Returns a Web Box and its status.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-webbox" method="get" path="/v1/webbox/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.get_webbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebBox](../../models/webbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_webbox

Renames a Web Box or replaces its metadata. Omitted fields keep their current values.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-webbox" method="patch" path="/v1/webbox/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.update_webbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | Web Box ID.                                                                                  |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New Web Box name.                                                                            |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.WebBox](../../models/webbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 404, 408, 413, 415  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_webbox_connection

Returns the URL that opens the Web Box's app, whether it is running or stopped. The app answers only while the Web Box is running.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-webbox-connection" method="get" path="/v1/webbox/{id}/connection" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.get_webbox_connection(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebboxConnection](../../models/webboxconnection.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## start_webbox

Starts a stopped Web Box from the state its stop saved, and returns at once with status starting. A running, starting or stopping Web Box returns 409 webbox_not_stopped.

### Example Usage

<!-- UsageSnippet language="python" operationID="start-webbox" method="post" path="/v1/webbox/{id}/start" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.start_webbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebBox](../../models/webbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409, 429            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## list_webbox_steps

Lists the Web Box's steps in order. Each request that changed state is one step; a request that only reads registers none. A Web Box that is not running returns 409 webbox_stopped.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-webbox-steps" method="get" path="/v1/webbox/{id}/steps" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.list_webbox_steps(id="<id>", limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box ID.                                                         |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListWebboxStepsResponse](../../models/listwebboxstepsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 404, 409, 422       | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## stop_webbox

Stops a running Web Box: it saves its state, and releases its runtime. It returns at once with status stopping; get the Web Box until it is stopped. A stopped Web Box is returned unchanged; a starting one returns 409 webbox_starting.

### Example Usage

<!-- UsageSnippet language="python" operationID="stop-webbox" method="post" path="/v1/webbox/{id}/stop" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox.stop_webbox(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box ID.                                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebBox](../../models/webbox.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |