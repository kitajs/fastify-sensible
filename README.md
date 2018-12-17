# fastify-sensible

[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](http://standardjs.com/)  [![Build Status](https://travis-ci.org/fastify/fastify-sensible.svg?branch=master)](https://travis-ci.org/fastify/fastify-sensible)

Defaults for Fastify that everyone can agree on™.<br>
This plugins adds some useful utilities to your Fastify instance, see the API section to learn more.

*Why these APIs are here and not directly into core?<br>
Because Fastify aims to be as small and focused as possible, every utility that is not essential should be shipped as standalone plugin.*

## Install
```
npm i fastify-sensible
```

## Usage
```js
const fastify = require('fastify')()
fastify.register(require('fastify-sensible'))

fastify.get('/', (req, reply) => {
  reply.notFound()
})

fastify.get('/async', async (req, reply) => {
  throw fastify.httpErrors.notFound()
})

fastify.listen(3000)
```
## API
#### `fastify.httpErrors`
Object that exposes all the `4xx` and `5xx` error constructors:
usage:
```js
 // the custom message is optional
const notFoundErr = fastify.httpErrors.notFound('custom message')
```
`4xx`
- <code>fastify.httpErrors.<b>badRequest()</b></code>
- <code>fastify.httpErrors.<b>unauthorized()</b></code>
- <code>fastify.httpErrors.<b>paymentRequired()</b></code>
- <code>fastify.httpErrors.<b>forbidden()</b></code>
- <code>fastify.httpErrors.<b>notFound()</b></code>
- <code>fastify.httpErrors.<b>methodNotAllowed()</b></code>
- <code>fastify.httpErrors.<b>notAcceptable()</b></code>
- <code>fastify.httpErrors.<b>proxyAuthenticationRequired()</b></code>
- <code>fastify.httpErrors.<b>requestTimeout()</b></code>
- <code>fastify.httpErrors.<b>conflict()</b></code>
- <code>fastify.httpErrors.<b>gone()</b></code>
- <code>fastify.httpErrors.<b>lengthRequired()</b></code>
- <code>fastify.httpErrors.<b>preconditionFailed()</b></code>
- <code>fastify.httpErrors.<b>payloadTooLarge()</b></code>
- <code>fastify.httpErrors.<b>uriTooLong()</b></code>
- <code>fastify.httpErrors.<b>unsupportedMediaType()</b></code>
- <code>fastify.httpErrors.<b>rangeNotSatisfiable()</b></code>
- <code>fastify.httpErrors.<b>expectationFailed()</b></code>
- <code>fastify.httpErrors.<b>imateapot()</b></code>
- <code>fastify.httpErrors.<b>misdirectedRequest()</b></code>
- <code>fastify.httpErrors.<b>unprocessableEntity()</b></code>
- <code>fastify.httpErrors.<b>locked()</b></code>
- <code>fastify.httpErrors.<b>failedDependency()</b></code>
- <code>fastify.httpErrors.<b>unorderedCollection()</b></code>
- <code>fastify.httpErrors.<b>upgradeRequired()</b></code>
- <code>fastify.httpErrors.<b>preconditionRequired()</b></code>
- <code>fastify.httpErrors.<b>tooManyRequests()</b></code>
- <code>fastify.httpErrors.<b>requestHeaderFieldsTooLarge()</b></code>
- <code>fastify.httpErrors.<b>unavailableForLegalReasons()</b></code>

`5xx`
- <code>fastify.httpErrors.<b>internalServerError()</b></code>
- <code>fastify.httpErrors.<b>notImplemented()</b></code>
- <code>fastify.httpErrors.<b>badGateway()</b></code>
- <code>fastify.httpErrors.<b>serviceUnavailable()</b></code>
- <code>fastify.httpErrors.<b>gatewayTimeout()</b></code>
- <code>fastify.httpErrors.<b>httpVersionNotSupported()</b></code>
- <code>fastify.httpErrors.<b>variantAlsoNegotiates()</b></code>
- <code>fastify.httpErrors.<b>insufficientStorage()</b></code>
- <code>fastify.httpErrors.<b>loopDetected()</b></code>
- <code>fastify.httpErrors.<b>bandwidthLimitExceeded()</b></code>
- <code>fastify.httpErrors.<b>notExtended()</b></code>
- <code>fastify.httpErrors.<b>networkAuthenticationRequired()</b></code>

#### `reply.[httpError]`
The `reply` interface is decorated with all the functions declared above, use it is very easy:
```js
fastify.get('/', (req, reply) => {
  reply.notFound()
})
```

#### `reply.vary`
The `reply` interface is decorated with [`jshttp/vary`](https://github.com/jshttp/vary), the API is the same, but you don't need to pass the res object.
```js
fastify.get('/', (req, reply) => {
  reply.vary('Accept')
  reply.send('ok')
})
```

#### `request.forwarded`
The `request` interface is decorated with [`jshttp/forwarded`](https://github.com/jshttp/forwarded), the API is the same, but you don't need to pass the request object.
```js
fastify.get('/', (req, reply) => {
  reply.send(req.forwarded())
})
```

#### `request.proxyaddr`
The `request` interface is decorated with [`jshttp/proxy-addr`](https://github.com/jshttp/proxy-addr), the API is the same, but you don't need to pass the request object.
```js
fastify.get('/', (req, reply) => {
  reply.send(req.proxyaddr(addr => addr === '127.0.0.1'))
})
```

#### `request.is`
The `request` interface is decorated with [`jshttp/type-is`](https://github.com/jshttp/type-is), the API is the same, but you don't need to pass the request object.
```js
fastify.get('/', (req, reply) => {
  reply.send(req.is(['html', 'json']))
})
```

#### `assert`
Verify if a given condition is true, if not it throws the specified http error.<br> Very useful if you work with *async* routes.
```js
// the custom message is optional
fastify.assert(
  req.headers.authorization, 400, 'Missing authorization header'
)
```
The `assert` API exposes also the following methods:
- <code>fastify.assert.<b>ok()</b></code>
- <code>fastify.assert.<b>equal()</b></code>
- <code>fastify.assert.<b>notEqual()</b></code>
- <code>fastify.assert.<b>strictEqual()</b></code>
- <code>fastify.assert.<b>notStrictEqual()</b></code>
- <code>fastify.assert.<b>deepEqual()</b></code>
- <code>fastify.assert.<b>notDeepEqual()</b></code>

#### `to`
Async await wrapper for easy error handling without try-catch, inspired by [`await-to-js`](https://github.com/scopsy/await-to-js)

```js
const [err, user] = await fastify.to(
  db.findOne({ user: 'tyrion' })
)
```

#### Custom error handler
This plugins also adds a custom error handler which hides the error message in case of `500` errors, instead it returns `Something went wrong`.<br>
This is especially useful if you are using *async* routes, where every uncaught error will be sent back to the user *(but dot not worry, the original error message is logged as error in any case)*.

## Contributing
Do you feel there is some utility that *everyone can agree on* which is not present?<br>
Open an issue and let's discuss it! Even better a pull request!

## Acknowledgements

The project name is inspired by [`vim-sensible`](https://github.com/tpope/vim-sensible), an awesome package that if you use vim you should use too.

## License

MIT
Copyright © 2018 Tomas Della Vedova
