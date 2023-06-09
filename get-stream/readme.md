# get-stream

> Get a stream as a string, buffer, or array

## Install

```
$ npm install get-stream
```

## Usage

```js
const fs = require('fs');
const getStream = require('get-stream');

(async () => {
	const stream = fs.createReadStream('unicorn.txt');

	console.log(await getStream(stream));
	/*
	              ,,))))))));,
	           __)))))))))))))),
	\|/       -\(((((''''((((((((.
	-*-==//////((''  .     `)))))),
	/|\      ))| o    ;-.    '(((((                                  ,(,
	         ( `|    /  )    ;))))'                               ,_))^;(~
	            |   |   |   ,))((((_     _____------~~~-.        %,;(;(>';'~
	            o_);   ;    )))(((` ~---~  `::           \      %%~~)(v;(`('~
	                  ;    ''''````         `:       `:::|\,__,%%    );`'; ~
	                 |   _                )     /      `:|`----'     `-'
	           ______/\/~    |                 /        /
	         /~;;.____/;;'  /          ___--,-(   `;;;/
	        / //  _;______;'------~~~~~    /;;/\    /
	       //  | |                        / ;   \;;,\
	      (<_  | ;                      /',/-----'  _>
	       \_| ||_                     //~;~~~~~~~~~
	           `\_|                   (,~~
	                                   \~\
	                                    ~~
	*/
})();
```

## API

The methods returns a promise that resolves when the `end` event fires on the stream, indicating that there is no more data to be read. The stream is switched to flowing mode.

### getStream(stream, options?)

Get the `stream` as a string.

#### options

Type: `object`

##### encoding

Type: `string`\
Default: `'utf8'`

[Encoding](https://nodejs.org/api/buffer.html#buffer_buffer) of the incoming stream.

##### maxBuffer

Type: `number`\
Default: `Infinity`

Maximum length of the returned string. If it exceeds this value before the stream ends, the promise will be rejected with a `getStream.MaxBufferError` error.

### getStream.buffer(stream, options?)

Get the `stream` as a buffer.

It honors the `maxBuffer` option as above, but it refers to byte length rather than string length.

### getStream.array(stream, options?)

Get the `stream` as an array of values.

It honors both the `maxBuffer` and `encoding` options. The behavior changes slightly based on the encoding chosen:

- When `encoding` is unset, it assumes an [object mode stream](https://nodesource.com/blog/understanding-object-streams/) and collects values emitted from `stream` unmodified. In this case `maxBuffer` refers to the number of items in the array (not the sum of their sizes).

- When `encoding` is set to `buffer`, it collects an array of buffers. `maxBuffer` refers to the summed byte lengths of every buffer in the array.

- When `encoding` is set to anything else, it collects an array of strings. `maxBuffer` refers to the summed character lengths of every string in the array.

## Errors

If the input stream emits an `error` event, the promise will be rejected with the error. The buffered data will be attached to the `bufferedData` property of the error.

```js
(async () => {
	try {
		await getStream(streamThatErrorsAtTheEnd('unicorn'));
	} catch (error) {
		console.log(error.bufferedData);
		//=> 'unicorn'
	}
})()
```

## FAQ

### How is this different from [`concat-stream`](https://github.com/maxogden/concat-stream)?

This module accepts a stream instead of being one and returns a promise instead of using a callback. The API is simpler and it only supports returning a string, buffer, or array. It doesn't have a fragile type inference. You explicitly choose what you want. And it doesn't depend on the huge `readable-stream` package.

## Related

- [get-stdin](https://github.com/sindresorhus/get-stdin) - Get stdin as a string or buffer

---

<div align="center">
	<b>
		<a href="https://tidelift.com/subscription/pkg/npm-get-stream?utm_source=npm-get-stream&utm_medium=referral&utm_campaign=readme">Get professional support for this package with a Tidelift subscription</a>
	</b>
	<br>
	<sub>
		Tidelift helps make open source sustainable for maintainers while giving companies<br>assurances about security, maintenance, and licensing for their dependencies.
	</sub>
</div>
