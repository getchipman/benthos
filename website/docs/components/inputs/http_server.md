---
title: http_server
type: input
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/input/http_server.go
-->


Receive messages POSTed over HTTP(S). HTTP 2.0 is supported when using TLS,
which is enabled when key and cert files are specified.


import Tabs from '@theme/Tabs';

<Tabs defaultValue="common" values={[
  { label: 'Common', value: 'common', },
  { label: 'Advanced', value: 'advanced', },
]}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

```yaml
# Common config fields, showing default values
input:
  http_server:
    address: ""
    path: /post
    ws_path: /post/ws
    timeout: 5s
    rate_limit: ""
```

</TabItem>
<TabItem value="advanced">

```yaml
# All config fields, showing default values
input:
  http_server:
    address: ""
    path: /post
    ws_path: /post/ws
    ws_welcome_message: ""
    ws_rate_limit_message: ""
    timeout: 5s
    rate_limit: ""
    cert_file: ""
    key_file: ""
    sync_response:
      headers:
        Content-Type: application/octet-stream
```

</TabItem>
</Tabs>

You can leave the 'address' config field blank in order to use the instance wide
HTTP server.

The field `rate_limit` allows you to specify an optional
[`rate_limit` resource](/docs/components/rate_limits/about), which
will be applied to each HTTP request made and each websocket payload received.

When the rate limit is breached HTTP requests will have a 429 response returned
with a Retry-After header. Websocket payloads will be dropped and an optional
response payload will be sent as per `ws_rate_limit_message`.

### Responses

It's possible to return a response for each message received using
[synchronous responses](/docs/guides/sync_responses). When doing so you can customise
headers with the `sync_response` field `headers`, which can also use
[function interpolation](/docs/configuration/interpolation#metadata) in the value based
on the response message contents.

### Endpoints

The following fields specify endpoints that are registered for sending messages:

#### `path` (defaults to `/post`)

This endpoint expects POST requests where the entire request body is consumed as
a single message.

If the request contains a multipart `content-type` header as per
[rfc1341](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html) then the
multiple parts are consumed as a batch of messages, where each body part is a
message of the batch.

#### `ws_path` (defaults to `/post/ws`)

Creates a websocket connection, where payloads received on the socket are passed
through the pipeline as a batch of one message.

You may specify an optional `ws_welcome_message`, which is a static
payload to be sent to all clients once a websocket connection is first
established.

It's also possible to specify a `ws_rate_limit_message`, which is a
static payload to be sent to clients that have triggered the servers rate limit.

### Metadata

This input adds the following metadata fields to each message:

``` text
- http_server_user_agent
- All headers (only first values are taken)
- All query parameters
- All cookies
```

You can access these metadata fields using
[function interpolation](/docs/configuration/interpolation#metadata).

## Fields

### `address`

An alternative address to host from. If left empty the service wide address is used.


Type: `string`  
Default: `""`  

### `path`

The endpoint path to listen for POST requests.


Type: `string`  
Default: `"/post"`  

### `ws_path`

The endpoint path to create websocket connections from.


Type: `string`  
Default: `"/post/ws"`  

### `ws_welcome_message`

An optional message to deliver to fresh websocket connections.


Type: `string`  
Default: `""`  

### `ws_rate_limit_message`

An optional message to delivery to websocket connections that are rate limited.


Type: `string`  
Default: `""`  

### `timeout`

Timeout for requests. If a consumed messages takes longer than this to be delivered the connection is closed, but the message may still be delivered.


Type: `string`  
Default: `"5s"`  

### `rate_limit`

An optional [rate limit](/docs/components/rate_limits/about) to throttle requests by.


Type: `string`  
Default: `""`  

### `cert_file`

Only valid with a custom `address`.


Type: `string`  
Default: `""`  

### `key_file`

Only valid with a custom `address`.


Type: `string`  
Default: `""`  

### `sync_response`

Customise messages returned via [synchronous responses](/docs/guides/sync_responses).


Type: `object`  
Default: `{"headers":{"Content-Type":"application/octet-stream"}}`  

### `sync_response.headers`

Specify headers to return with synchronous responses.
This field supports [interpolation functions](/docs/configuration/interpolation#bloblang-queries).


Type: `object`  
Default: `{"Content-Type":"application/octet-stream"}`  


