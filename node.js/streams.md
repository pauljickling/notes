## Streams

Node.js provides an API for interacting with streaming data.

`const stream = require('stream');`

There are four types of streams:

1. Readable: streams with readable data
2. Writable: streams that an application can write to
3. Duplex: streams that are readable and writable
4. Transform: streams that modify or transform the data as it is written and read. A transform stream's output is related to its input.

By default Node streams operate on strings and buffer objects, however there is an optional parameter to work with other Javascript values.

```
let readable = new stream.Readable({
  encoding: 'UTF-8',
  highWaterMark: 16000,
  objectMode: true
});
```
