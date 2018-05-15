---
title: Response API
permalink: /docs/response-api/
---

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [Response](#response)
    -   [cache](#cache)
    -   [noCache](#nocache)
    -   [charSet](#charset)
    -   [header](#header)
    -   [json](#json)
    -   [link](#link)
    -   [send](#send)
    -   [sendRaw](#sendraw)
    -   [set](#set)
    -   [status](#status)
    -   [redirect](#redirect)
    -   [redirect](#redirect-1)
    -   [redirect](#redirect-2)

## Response

**Extends http.ServerResponse**

Wraps all of the node
[http.ServerResponse](https://nodejs.org/docs/latest/api/http.html)
APIs, events and properties, plus the following.

### cache

Sets the `cache-control` header.

**Parameters**

-   `type` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** value of the header
                                       (`"public"` or `"private"`) (optional, default `"public"`)
-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** an options object
    -   `options.maxAge` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** max-age in seconds

Returns **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** the value set to the header

### noCache

Turns off all cache related headers.

Returns **[Response](#response)** self, the response object

### charSet

Appends the provided character set to the response's `Content-Type`.

**Parameters**

-   `type` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** char-set value

**Examples**

```javascript
res.charSet('utf-8');
```

Returns **[Response](#response)** self, the response object

### header

Sets headers on the response.

**Parameters**

-   `key` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** the name of the header
-   `value` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** the value of the header

**Examples**

_If only key is specified, return the value of the header.
If both key and value are specified, set the response header._

```javascript
res.header('Content-Length');
// => undefined

res.header('Content-Length', 123);
// => 123

res.header('Content-Length');
// => 123

res.header('foo', new Date());
// => Fri, 03 Feb 2012 20:09:58 GMT
```

_`header()` can also be used to automatically chain header values
when applicable:_

```javascript
res.header('x-foo', 'a');
res.header('x-foo', 'b');
// => { 'x-foo': ['a', 'b'] }
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** the retrieved value or the value that was set

### json

Syntatic sugar for:

```js
res.contentType = 'json';
res.send({hello: 'world'});
```

**Parameters**

-   `code` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?**    http status code
-   `body` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?**    value to json.stringify
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** headers to set on the response

**Examples**

```javascript
res.header('content-type', 'json');
res.send({hello: 'world'});
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** the response object

### link

Sets the link header.

**Parameters**

-   `key` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**  the link key
-   `value` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** the link value

Returns **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** the header value set to res

### send

Sends the response object. pass through to internal `__send` that uses a
formatter based on the `content-type` header.

**Parameters**

-   `code` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** http status code
-   `body` **([Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object) \| [Buffer](https://nodejs.org/api/buffer.html) \| [Error](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error))?** the content to send
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** any add'l headers to set

**Examples**

_You can use send() to wrap up all the usual writeHead(), write(), end()
calls on the HTTP API of node.
You can pass send either a `code` and `body`, or just a body. body can be
an `Object`, a `Buffer`, or an `Error`.
When you call `send()`, restify figures out how to format the response
based on the `content-type`._

```javascript
res.send({hello: 'world'});
res.send(201, {hello: 'world'});
res.send(new BadRequestError('meh'));
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** the response object

### sendRaw

Like `res.send()`, but skips formatting. This can be useful when the
payload has already been preformatted.
Sends the response object. pass through to internal `__send` that skips
formatters entirely and sends the content as is.

**Parameters**

-   `code` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** http status code
-   `body` **([Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object) \| [Buffer](https://nodejs.org/api/buffer.html) \| [Error](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error))?** the content to send
-   `headers` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** any add'l headers to set

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** the response object

### set

Sets multiple header(s) on the response.
Uses `header()` underneath the hood, enabling multi-value headers.

**Parameters**

-   `name` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object))** name of the header or
                                   `Object` of headers
-   `val` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** value of the header

**Examples**

```javascript
res.header('x-foo', 'a');
res.set({
    'x-foo', 'b',
    'content-type': 'application/json'
});
// =>
// {
//    'x-foo': [ 'a', 'b' ],
//    'content-type': 'application/json'
// }
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** self, the response object

### status

Sets the http status code on the response.

**Parameters**

-   `code` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** http status code

**Examples**

```javascript
res.status(201);
```

Returns **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** the status code passed in

### redirect

Redirect is sugar method for redirecting.

**Parameters**

-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** url or an options object to configure a redirect
    -   `options.secure` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** whether to redirect to http or https
    -   `options.hostname` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** redirect location's hostname
    -   `options.pathname` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** redirect location's pathname
    -   `options.port` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** redirect location's port number
    -   `options.query` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** redirect location's query string
                                        parameters
    -   `options.overrideQuery` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if true, `options.query`
                                                 stomps over any existing query
                                                 parameters on current URL.
                                                 by default, will merge the two.
    -   `options.permanent` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?** if true, sets 301. defaults to 302.
-   `next` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** mandatory, to complete the response and trigger
                           audit logger.

**Examples**

```javascript
res.redirect({...}, next);
```

_A convenience method for 301/302 redirects. Using this method will tell
restify to stop execution of your handler chain.
You can also use an options object. `next` is required._

```javascript
res.redirect({
  hostname: 'www.foo.com',
  pathname: '/bar',
  port: 80,                 // defaults to 80
  secure: true,             // sets https
  permanent: true,
  query: {
    a: 1
  }
}, next);  // => redirects to 301 https://www.foo.com/bar?a=1
```

Returns **[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)** 

### redirect

Redirect with code and url.

**Parameters**

-   `code` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** http redirect status code
-   `url` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** redirect url
-   `next` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** mandatory, to complete the response and trigger
                           audit logger.

**Examples**

```javascript
res.redirect(301, 'www.foo.com', next);
```

Returns **[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)** 

### redirect

Redirect with url.

**Parameters**

-   `url` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** redirect url
-   `next` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** mandatory, to complete the response and trigger
                           audit logger.

**Examples**

```javascript
res.redirect('www.foo.com', next);
res.redirect('/foo', next);
```

Returns **[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)** 