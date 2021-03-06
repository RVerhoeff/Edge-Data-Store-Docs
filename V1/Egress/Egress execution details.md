---
uid: EgressExecutionDetails
---

# Egress execution details

After an egress endpoint is configured, egress execution occurs periodically for that endpoint. Egress is handled individually per configured endpoint. Only the first execution types and containers are egressed; subsequently, only new or changed types/containers are egressed. 

**Note:** Only streams with a single, timeseries-based index can be egressed. 

Type creation must be successful to perform container creation, and container creation must be successful to perform data egress.

Type, container, and data items are batched into one or more OMF messages when egressing. Per the requirements defined in the OMF specification, a single message will not exceed 192KB in size. Compression is automatically applied to outbound egress messages. On the destination, failure to add a single item will result in the message failing. In that case the Edge Data Store will fall back to egressing each item individually, per type or stream (that is each type, each stream, all data for a single stream). Types, containers, and data will continue to be egressed as long as the destination continues to respond to HTTP requests - retrying previous failures as needed.

If egress fails with one of the following errors, EDS will retry the request up to five times:

- TimeoutException
- HttpRequestException
- HttpStatusCode RequestTimeout (408)
- HttpStatusCode BadGateway (502)
- HttpStatusCode ServiceUnavailable (503)
- HttpStatusCode GatewayTimeout (504)

There is an increasing delay before each retry. The total time for all five retries is one minute; during which time, all other egress messages for this definition are queued. If a TimeoutException or HttpRequestException error still occurs at the fifth retry, the egress fails, and EDS waits five minutes before trying the whole process again. If one of the other errors still occurs at the fifth retry, the message fails and EDS sends the next message in the queue.

For data collection and egress, in-memory and on-disk storage are used to track the last successfully egressed data event, per stream. Data is egressed in order and includes future events.

**Note:**  When an event with a future timestamp is successfully egressed, only values after the associated timestamp of that event will be egressed.
