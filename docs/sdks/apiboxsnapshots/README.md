# ApiboxSnapshots

## Overview

Build API Box Snapshots from an OpenAPI, WSDL or GraphQL document, or use the catalog's. API Boxes start from a ready Snapshot.

### Available Operations

* [list_apibox_snapshots](#list_apibox_snapshots) - List API Box Snapshots
* [create_apibox_snapshot](#create_apibox_snapshot) - Create API Box Snapshot
* [delete_apibox_snapshot](#delete_apibox_snapshot) - Delete API Box Snapshot
* [get_apibox_snapshot](#get_apibox_snapshot) - Get API Box Snapshot
* [update_apibox_snapshot](#update_apibox_snapshot) - Update API Box Snapshot
* [cancel_apibox_snapshot](#cancel_apibox_snapshot) - Cancel Snapshot build
* [resume_apibox_snapshot](#resume_apibox_snapshot) - Resume Snapshot build

## list_apibox_snapshots

Returns the workspace's API Box Snapshots and the catalog's, newest first. Results can be filtered by source and status.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-apibox-snapshots" method="get" path="/v1/apibox-snapshots" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.list_apibox_snapshots(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                                               | Type                                                                                    | Required                                                                                | Description                                                                             |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `status`                                                                                | [Optional[models.ListApiboxSnapshotsStatus]](../../models/listapiboxsnapshotsstatus.md) | :heavy_minus_sign:                                                                      | Optional build status filter.                                                           |
| `limit`                                                                                 | *Optional[int]*                                                                         | :heavy_minus_sign:                                                                      | Page size. Values below 1 use 50. Values above 200 use 200.                             |
| `cursor`                                                                                | *Optional[str]*                                                                         | :heavy_minus_sign:                                                                      | Opaque next_cursor value from a previous page.                                          |
| `source`                                                                                | [Optional[models.ListApiboxSnapshotsSource]](../../models/listapiboxsnapshotssource.md) | :heavy_minus_sign:                                                                      | Optional source filter.                                                                 |
| `retries`                                                                               | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                        | :heavy_minus_sign:                                                                      | Configuration to override the default retry behavior of the client.                     |

### Response

**[models.ListApiboxSnapshotsResponseResponse](../../models/listapiboxsnapshotsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_apibox_snapshot

Starts an asynchronous build and returns the API Box Snapshot with status pending. Get the Snapshot until its status is ready, failed or canceled; API Boxes start from a ready one. The build is a Simulator build in api mode, run in real time: the built service has no simulated clock, and its scheduled work runs at the first request at or after its time. Builds queue with Simulator builds and count toward the same quotas. Send multipart/form-data with a JSON part named request, a file part named spec with the OpenAPI, WSDL or GraphQL document, and optional file parts named document with vendor documentation.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-apibox-snapshot" method="post" path="/v1/apibox-snapshots" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.create_apibox_snapshot(request={})

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                                         | Type                                                                                                                                                                                                                                                                                                                                              | Required                                                                                                                                                                                                                                                                                                                                          | Description                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request`                                                                                                                                                                                                                                                                                                                                         | [models.BuildAPIBoxSnapshotRequest](../../models/buildapiboxsnapshotrequest.md)                                                                                                                                                                                                                                                                   | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                | N/A                                                                                                                                                                                                                                                                                                                                               |
| `idempotency_key`                                                                                                                                                                                                                                                                                                                                 | *Optional[str]*                                                                                                                                                                                                                                                                                                                                   | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Optional key for retrying this create safely. A retry with the same key and request returns the original result with its status; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new resource. The key is recorded before the build starts, so a retry never starts a second build. |
| `document`                                                                                                                                                                                                                                                                                                                                        | List[[models.CreateApiboxSnapshotDocument](../../models/createapiboxsnapshotdocument.md)]                                                                                                                                                                                                                                                         | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Vendor documentation the builder and reviewer read before they start, as repeated file parts: UTF-8 text (.md, .txt, .html, .json, .yaml, .xml, .csv) or PDF (.pdf). The documents share the request body limit with the spec.                                                                                                                    |
| `spec`                                                                                                                                                                                                                                                                                                                                            | [Optional[models.CreateApiboxSnapshotSpec]](../../models/createapiboxsnapshotspec.md)                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | OpenAPI, WSDL or GraphQL document, UTF-8 encoded, at most 64 MiB.                                                                                                                                                                                                                                                                                 |
| `retries`                                                                                                                                                                                                                                                                                                                                         | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                                                                                                  | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                                                                                               |

### Response

**[models.CreateApiboxSnapshotResponse](../../models/createapiboxsnapshotresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 408, 413, 415, 422  | application/problem+json      |
| errors.Error                  | 429                           | application/problem+json      |
| errors.Error                  | 500                           | application/problem+json      |
| errors.Error                  | 503                           | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## delete_apibox_snapshot

Deletes an API Box Snapshot. A build that is still pending or building is canceled first. A Snapshot that API Boxes started from or MCP Box Snapshots were built against returns 409 apibox_snapshot_in_use; delete those first. Catalog Snapshots cannot be deleted.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-apibox-snapshot" method="delete" path="/v1/apibox-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.apibox_snapshots.delete_apibox_snapshot(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_apibox_snapshot

Returns an API Box Snapshot of the workspace or the catalog, and its build status.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-apibox-snapshot" method="get" path="/v1/apibox-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.get_apibox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.APIBoxSnapshot](../../models/apiboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_apibox_snapshot

Renames an API Box Snapshot or replaces its metadata. Omitted fields keep their current values. The built service does not change. Catalog Snapshots cannot be updated.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-apibox-snapshot" method="patch" path="/v1/apibox-snapshots/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.update_apibox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | API Box Snapshot ID.                                                                         |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New Snapshot name.                                                                           |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.APIBoxSnapshot](../../models/apiboxsnapshot.md)**

### Errors

| Error Type                        | Status Code                       | Content Type                      |
| --------------------------------- | --------------------------------- | --------------------------------- |
| errors.Error                      | 400, 401, 403, 404, 408, 413, 415 | application/problem+json          |
| errors.Error                      | 500, 503                          | application/problem+json          |
| errors.ContinuousDefaultError     | 4XX, 5XX                          | \*/\*                             |

## cancel_apibox_snapshot

Requests cancellation of a pending or active build and returns the Snapshot without waiting for the build to stop. Get the Snapshot until its status leaves pending or building. The build can finish before cancellation takes effect. A Snapshot whose build has settled is returned unchanged.

### Example Usage

<!-- UsageSnippet language="python" operationID="cancel-apibox-snapshot" method="post" path="/v1/apibox-snapshots/{id}/cancel" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.cancel_apibox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.APIBoxSnapshot](../../models/apiboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## resume_apibox_snapshot

Resumes a build that waits at QA because its live credentials were rejected, as build.waiting reports. Refresh each credential it names with PATCH /v1/credentials/{id} first: QA then runs again with their current values, and the build goes on to finalize. Returns the Snapshot, still building. The wait does not count toward the build's time limit: the build resumes with the time it had left when it began waiting. A build that is not waiting returns 409 simulator_not_waiting.

### Example Usage

<!-- UsageSnippet language="python" operationID="resume-apibox-snapshot" method="post" path="/v1/apibox-snapshots/{id}/resume" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.resume_apibox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | API Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.APIBoxSnapshot](../../models/apiboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |