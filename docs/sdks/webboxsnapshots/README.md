# WebboxSnapshots

## Overview

Build Web Box Snapshots from a vendor's live web app, or use the catalog's. Web Boxes start from a ready Snapshot.

### Available Operations

* [list_webbox_snapshots](#list_webbox_snapshots) - List Web Box Snapshots
* [create_webbox_snapshot](#create_webbox_snapshot) - Create Web Box Snapshot
* [delete_webbox_snapshot](#delete_webbox_snapshot) - Delete Web Box Snapshot
* [get_webbox_snapshot](#get_webbox_snapshot) - Get Web Box Snapshot
* [update_webbox_snapshot](#update_webbox_snapshot) - Update Web Box Snapshot
* [cancel_webbox_snapshot](#cancel_webbox_snapshot) - Cancel Snapshot build
* [resume_webbox_snapshot](#resume_webbox_snapshot) - Resume Snapshot build

## list_webbox_snapshots

Returns the workspace's Web Box Snapshots and the catalog's, newest first. Results can be filtered by source and status.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-webbox-snapshots" method="get" path="/v1/webbox-snapshots" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.list_webbox_snapshots(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                                               | Type                                                                                    | Required                                                                                | Description                                                                             |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `status`                                                                                | [Optional[models.ListWebboxSnapshotsStatus]](../../models/listwebboxsnapshotsstatus.md) | :heavy_minus_sign:                                                                      | Optional build status filter.                                                           |
| `limit`                                                                                 | *Optional[int]*                                                                         | :heavy_minus_sign:                                                                      | Page size. Values below 1 use 50. Values above 200 use 200.                             |
| `cursor`                                                                                | *Optional[str]*                                                                         | :heavy_minus_sign:                                                                      | Opaque next_cursor value from a previous page.                                          |
| `source`                                                                                | [Optional[models.ListWebboxSnapshotsSource]](../../models/listwebboxsnapshotssource.md) | :heavy_minus_sign:                                                                      | Optional source filter.                                                                 |
| `retries`                                                                               | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                        | :heavy_minus_sign:                                                                      | Configuration to override the default retry behavior of the client.                     |

### Response

**[models.ListWebboxSnapshotsResponseResponse](../../models/listwebboxsnapshotsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_webbox_snapshot

Starts an asynchronous build and returns the Web Box Snapshot with status pending. Get the Snapshot until its status is ready, failed or canceled; Web Boxes start from a ready one. The build is a Web App build, run in real time: the builder and QA use the vendor's live web app through the Web App login credential, and the built app has no simulated clock. Builds queue with Simulator builds and count toward the same quotas. Send multipart/form-data with a JSON part named request and optional file parts named document with vendor documentation.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-webbox-snapshot" method="post" path="/v1/webbox-snapshots" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.create_webbox_snapshot(request={
        "credential_ids": [
            "<value 1>",
            "<value 2>",
            "<value 3>",
        ],
    })

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                                         | Type                                                                                                                                                                                                                                                                                                                                              | Required                                                                                                                                                                                                                                                                                                                                          | Description                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request`                                                                                                                                                                                                                                                                                                                                         | [models.BuildWebBoxSnapshotRequest](../../models/buildwebboxsnapshotrequest.md)                                                                                                                                                                                                                                                                   | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                | N/A                                                                                                                                                                                                                                                                                                                                               |
| `idempotency_key`                                                                                                                                                                                                                                                                                                                                 | *Optional[str]*                                                                                                                                                                                                                                                                                                                                   | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Optional key for retrying this create safely. A retry with the same key and request returns the original result with its status; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new resource. The key is recorded before the build starts, so a retry never starts a second build. |
| `document`                                                                                                                                                                                                                                                                                                                                        | List[[models.CreateWebboxSnapshotDocument](../../models/createwebboxsnapshotdocument.md)]                                                                                                                                                                                                                                                         | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Vendor documentation the builder and reviewer read before they start, as repeated file parts: UTF-8 text (.md, .txt, .html, .json, .yaml, .xml, .csv) or PDF (.pdf). The documents share the request body limit.                                                                                                                                  |
| `retries`                                                                                                                                                                                                                                                                                                                                         | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                                                                                                  | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                                                                                               |

### Response

**[models.CreateWebboxSnapshotResponse](../../models/createwebboxsnapshotresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 408, 413, 415, 422  | application/problem+json      |
| errors.Error                  | 429                           | application/problem+json      |
| errors.Error                  | 500                           | application/problem+json      |
| errors.Error                  | 503                           | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## delete_webbox_snapshot

Deletes a Web Box Snapshot. A build that is still pending or building is canceled first. A Snapshot that Web Boxes started from returns 409 webbox_snapshot_in_use; delete those Web Boxes first. Catalog Snapshots cannot be deleted.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-webbox-snapshot" method="delete" path="/v1/webbox-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.webbox_snapshots.delete_webbox_snapshot(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_webbox_snapshot

Returns a Web Box Snapshot of the workspace or the catalog, and its build status.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-webbox-snapshot" method="get" path="/v1/webbox-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.get_webbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebBoxSnapshot](../../models/webboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_webbox_snapshot

Renames a Web Box Snapshot or replaces its metadata. Omitted fields keep their current values. The built app does not change. Catalog Snapshots cannot be updated.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-webbox-snapshot" method="patch" path="/v1/webbox-snapshots/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.update_webbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | Web Box Snapshot ID.                                                                         |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New Snapshot name.                                                                           |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.WebBoxSnapshot](../../models/webboxsnapshot.md)**

### Errors

| Error Type                        | Status Code                       | Content Type                      |
| --------------------------------- | --------------------------------- | --------------------------------- |
| errors.Error                      | 400, 401, 403, 404, 408, 413, 415 | application/problem+json          |
| errors.Error                      | 500, 503                          | application/problem+json          |
| errors.ContinuousDefaultError     | 4XX, 5XX                          | \*/\*                             |

## cancel_webbox_snapshot

Requests cancellation of a pending or active build and returns the Snapshot without waiting for the build to stop. Get the Snapshot until its status leaves pending or building. The build can finish before cancellation takes effect. A Snapshot whose build has settled is returned unchanged.

### Example Usage

<!-- UsageSnippet language="python" operationID="cancel-webbox-snapshot" method="post" path="/v1/webbox-snapshots/{id}/cancel" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.cancel_webbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebBoxSnapshot](../../models/webboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## resume_webbox_snapshot

Resumes a build that waits at QA because its live credentials were rejected, as build.waiting reports. Refresh each credential it names first: QA then runs again with their current values, and the build goes on to finalize. Returns the Snapshot, still building. The wait does not count toward the build's time limit. A build that is not waiting returns 409 simulator_not_waiting.

### Example Usage

<!-- UsageSnippet language="python" operationID="resume-webbox-snapshot" method="post" path="/v1/webbox-snapshots/{id}/resume" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.resume_webbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Web Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WebBoxSnapshot](../../models/webboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |