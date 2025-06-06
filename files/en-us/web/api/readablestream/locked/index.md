---
title: "ReadableStream: locked property"
short-title: locked
slug: Web/API/ReadableStream/locked
page-type: web-api-instance-property
browser-compat: api.ReadableStream.locked
---

{{APIRef("Streams")}}{{AvailableInWorkers}}

The **`locked`** read-only property of the {{domxref("ReadableStream")}} interface returns whether or not the readable stream is locked to a reader.

A readable stream can have at most one active reader at a time, and is locked to that reader until it is released.
A reader might be obtained using [`ReadableStream.getReader()`](/en-US/docs/Web/API/ReadableStream/getReader) and released using the reader's `releaseLock()` method.

## Value

A {{Glossary("boolean")}} value indicating whether or not the readable stream is locked.

## Examples

```js
const stream = new ReadableStream({
  // …
});

const reader = stream.getReader();

stream.locked;
// should return true, as the stream has been locked to a reader
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ReadableStream.ReadableStream", "ReadableStream()")}} constructor
- [Using readable streams](/en-US/docs/Web/API/Streams_API/Using_readable_streams)
