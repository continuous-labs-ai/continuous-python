# Credentials

## Overview

Store connections to real systems: a base URL, the header that carries the credential, and its value. The API never returns a value.

### Available Operations

* [list_credentials](#list_credentials) - List credentials
* [create_credential](#create_credential) - Create credential
* [delete_credential](#delete_credential) - Delete credential
* [get_credential](#get_credential) - Get credential
* [update_credential](#update_credential) - Update credential

## list_credentials

Returns the workspace's credentials, newest first. Credential values are never returned.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-credentials" method="get" path="/v1/credentials" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.credentials.list_credentials(limit=50)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListCredentialsResponse](../../models/listcredentialsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_credential

Stores a connection to a real system: its base URL, the header that carries the credential, and the header value. Returns the credential without its value.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-credential" method="post" path="/v1/credentials" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.credentials.create_credential(base_url="https://api.stripe.com", name="stripe-test", value="Bearer <redacted>", header="Authorization")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                | Type                                                                                     | Required                                                                                 | Description                                                                              |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `base_url`                                                                               | *str*                                                                                    | :heavy_check_mark:                                                                       | The https URL of the real system. Requests go to paths under it.                         |
| `name`                                                                                   | *str*                                                                                    | :heavy_check_mark:                                                                       | Credential name.                                                                         |
| `value`                                                                                  | *str*                                                                                    | :heavy_check_mark:                                                                       | The full header value, for example Bearer followed by a token. The API never returns it. |
| `header`                                                                                 | *Optional[str]*                                                                          | :heavy_minus_sign:                                                                       | The HTTP header that carries the value.                                                  |
| `retries`                                                                                | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                         | :heavy_minus_sign:                                                                       | Configuration to override the default retry behavior of the client.                      |

### Response

**[models.Credential](../../models/credential.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 408, 413, 415, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## delete_credential

Deletes a credential and its stored value.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-credential" method="delete" path="/v1/credentials/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.credentials.delete_credential(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Credential ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_credential

Returns a credential without its value.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-credential" method="get" path="/v1/credentials/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.credentials.get_credential(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Credential ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Credential](../../models/credential.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_credential

Renames a credential, changes its base URL or header, or replaces its value. Omitted fields keep their current values.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-credential" method="patch" path="/v1/credentials/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.credentials.update_credential(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                      | Type                                                                                                                                           | Required                                                                                                                                       | Description                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                           | *str*                                                                                                                                          | :heavy_check_mark:                                                                                                                             | Credential ID.                                                                                                                                 |
| `base_url`                                                                                                                                     | *Optional[str]*                                                                                                                                | :heavy_minus_sign:                                                                                                                             | New https URL of the real system.                                                                                                              |
| `header`                                                                                                                                       | *Optional[str]*                                                                                                                                | :heavy_minus_sign:                                                                                                                             | New HTTP header that carries the value.                                                                                                        |
| `name`                                                                                                                                         | *Optional[str]*                                                                                                                                | :heavy_minus_sign:                                                                                                                             | New credential name.                                                                                                                           |
| `value`                                                                                                                                        | *Optional[str]*                                                                                                                                | :heavy_minus_sign:                                                                                                                             | New full header value. Required when base_url or header changes. Builds that start an agent after the change use it. The API never returns it. |
| `retries`                                                                                                                                      | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                               | :heavy_minus_sign:                                                                                                                             | Configuration to override the default retry behavior of the client.                                                                            |

### Response

**[models.Credential](../../models/credential.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 403, 404, 408, 413, 415, 422 | application/problem+json               |
| errors.Error                           | 500, 503                               | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |