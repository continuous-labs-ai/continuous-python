# WebboxSnapshots

## Overview

Build Web Box Snapshots from a vendor's live web app, or use the catalog's. Web Boxes start from a ready Snapshot.

### Available Operations

* [list_webbox_snapshots](#list_webbox_snapshots) - List Web Box Snapshots
* [create_webbox_snapshot](#create_webbox_snapshot) - Create Web Box Snapshot
* [delete_webbox_snapshot](#delete_webbox_snapshot) - Delete Web Box Snapshot
* [get_webbox_snapshot](#get_webbox_snapshot) - Get Web Box Snapshot
* [update_webbox_snapshot](#update_webbox_snapshot) - Update Web Box Snapshot
* [cancel_webbox_snapshot](#cancel_webbox_snapshot) - Cancel Web Box Snapshot build
* [clone_webbox_snapshot](#clone_webbox_snapshot) - Clone Web Box Snapshot
* [resume_webbox_snapshot](#resume_webbox_snapshot) - Resume Web Box Snapshot build

## list_webbox_snapshots

Returns the workspace's Web Box Snapshots and the catalog's, newest first. Results can be filtered by source and status, and by the Web Box a Snapshot was saved from.

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
| `box_id`                                                                                | *Optional[str]*                                                                         | :heavy_minus_sign:                                                                      | Optional filter for the Snapshots saved from this Box.                                  |
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

Starts an asynchronous build and returns the Web Box Snapshot with status pending. Get the Snapshot until its status is ready, failed or canceled; Web Boxes start from a ready one. The build runs in real time: the builder and QA use the vendor's live web app through its login credential, and the built app has no simulated clock. Builds queue with Simulator builds and count toward the same quotas. Send multipart/form-data with a JSON part named request and optional file parts named document with vendor documentation. To build on a ready Snapshot, set parent_id.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-webbox-snapshot" method="post" path="/v1/webbox-snapshots" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.create_webbox_snapshot(request={})

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

Deletes a Web Box Snapshot. A build that is still pending or building is canceled first. A Snapshot that Web Boxes started from returns 409 webbox_snapshot_in_use; delete those Web Boxes first. While a build that names it as parent_id is pending or building, it returns 409 webbox_snapshot_parent_in_use; retry after the build finishes. Catalog Snapshots cannot be deleted.

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

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 403, 404, 408, 413, 415, 422 | application/problem+json               |
| errors.Error                           | 500, 503                               | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |

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

## clone_webbox_snapshot

Creates a ready copy of a ready Web Box Snapshot in another workspace, without a build. The clone has a new ID and the source's artifact, documents and saved state; the source is unchanged. The clone records no credentials, since they belong to the source workspace, and no parent or source Snapshot. The source is the caller's workspace: an API key's own workspace, or X-Workspace-Id for a session. An API key may clone into any other workspace of its organization.

### Example Usage: bad_request_body

<!-- UsageSnippet language="python" operationID="clone-webbox-snapshot" method="post" path="/v1/webbox-snapshots/{id}/clone" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.clone_webbox_snapshot(id="<id>", target_workspace_id="<id>")

    # Handle response
    print(res)

```
### Example Usage: bad_request_clone_target

<!-- UsageSnippet language="python" operationID="clone-webbox-snapshot" method="post" path="/v1/webbox-snapshots/{id}/clone" example="bad_request_clone_target" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.webbox_snapshots.clone_webbox_snapshot(id="<id>", target_workspace_id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                         | Type                                                                                                                                                                                                                                                                              | Required                                                                                                                                                                                                                                                                          | Description                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                                                                                                                                                              | *str*                                                                                                                                                                                                                                                                             | :heavy_check_mark:                                                                                                                                                                                                                                                                | Source Snapshot ID.                                                                                                                                                                                                                                                               |
| `target_workspace_id`                                                                                                                                                                                                                                                             | *str*                                                                                                                                                                                                                                                                             | :heavy_check_mark:                                                                                                                                                                                                                                                                | Workspace that receives the clone. It must differ from the source workspace.                                                                                                                                                                                                      |
| `idempotency_key`                                                                                                                                                                                                                                                                 | *Optional[str]*                                                                                                                                                                                                                                                                   | :heavy_minus_sign:                                                                                                                                                                                                                                                                | Optional key for retrying this create safely. A retry with the same key and request returns the original clone; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new clone. Keys are scoped to the target workspace. |
| `name`                                                                                                                                                                                                                                                                            | *Optional[str]*                                                                                                                                                                                                                                                                   | :heavy_minus_sign:                                                                                                                                                                                                                                                                | Name for the clone. Defaults to the source Snapshot's name.                                                                                                                                                                                                                       |
| `retries`                                                                                                                                                                                                                                                                         | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                                  | :heavy_minus_sign:                                                                                                                                                                                                                                                                | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                               |

### Response

**[models.WebBoxSnapshot](../../models/webboxsnapshot.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 422 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## resume_webbox_snapshot

Resumes a build that waits, as build.waiting reports: at QA because its live credentials were rejected or the live systems rate limited its calls, or for more time after it used its time limit. For a credential wait, refresh each credential it names with PATCH /v1/credentials/{id} first: QA then runs again with their current values, and the build goes on to finalize. For a rate-limit wait, resume once the rate limit resets: QA then runs again. For a wait for more time, send add_seconds: the build continues where it stopped, with that much time from the resume. Returns the Snapshot, still building. A wait does not count toward the build's time limit: a build resumed from a QA wait has the time it had left when it began waiting. A build that is not waiting returns 409 webbox_snapshot_not_waiting.

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

| Parameter                                                                                                                                                                                                                                                                                        | Type                                                                                                                                                                                                                                                                                             | Required                                                                                                                                                                                                                                                                                         | Description                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `id`                                                                                                                                                                                                                                                                                             | *str*                                                                                                                                                                                                                                                                                            | :heavy_check_mark:                                                                                                                                                                                                                                                                               | Snapshot ID.                                                                                                                                                                                                                                                                                     |
| `add_seconds`                                                                                                                                                                                                                                                                                    | *Optional[int]*                                                                                                                                                                                                                                                                                  | :heavy_minus_sign:                                                                                                                                                                                                                                                                               | Seconds to add to a build that waits for more time, from 1 to the largest timeout_seconds a build request accepts. Required for a build whose build.waiting.type is time_limit, and refused for any other wait. The build's time limit with every addition is at most 259200 seconds (72 hours). |
| `retries`                                                                                                                                                                                                                                                                                        | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                                                 | :heavy_minus_sign:                                                                                                                                                                                                                                                                               | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                                              |

### Response

**[models.WebBoxSnapshot](../../models/webboxsnapshot.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 403, 404, 408, 409, 413, 415 | application/problem+json               |
| errors.Error                           | 500, 503                               | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |