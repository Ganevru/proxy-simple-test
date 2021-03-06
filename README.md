# Proxy Simple Test

[![Build Status](https://travis-ci.com/Ganevru/proxy-simple-test.svg?branch=master)](https://travis-ci.com/Ganevru/proxy-simple-test)
[![npm](https://img.shields.io/npm/v/proxy-simple-test.svg?style=flat-square)](http://npm.im/proxy-simple-test)

Simple proxy testing.

All work is done by [node-tunnel](https://github.com/koichik/node-tunnel) and [got](https://github.com/sindresorhus/got) - this is a great library that can do a lot, use it if you need more.

In order to make an object from a proxy string, I use my library - [split-proxy](https://github.com/Ganevru/split-proxy)

`proxy-simple-test` - always returns `true` if test is passed or `false` if not.

The first argument is a proxy as a string or as an object (use what is convenient for you).
The second argument is the webpage to check.

The third argument is optional; it checks the body of the response to a specific text.

`inBody` - if body of the answer has this text then the test is passed.

`notInBody` - if body of the answer has this text then the test is NOT passed.

If third argument is a string, then it is automatically considered as `inBody`.

The test never passes if the response code is NOT `200`.

It is not necessary to use a proxy with `login` and `password`.

At the moment - only HTTP over HTTP tunneling.

## Install

```bash
npm i proxy-simple-test
```

## Examples

Proxy string:

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    '123.123.2.42:8080@superLogin:superPassword',
    'www.example.com',
    { inBody: '<h1>Example Domain</h1>', notInBody: '<h1>404</h1>' }
  );
})();

// return true or false
```

The third argument can be a string, then it will automatically be used as `inBody`.

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    '123.123.2.42:8080@superLogin:superPassword',
    'www.example.com',
    '<h1>Example Domain</h1>'
  );
})();

// return true or false
```

Proxy string, without defining text from the body, in this case returns `true` if response code is `200`:

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    '123.123.2.42:8080@superLogin:superPassword',
    'www.example.com'
  );
})();

// return true or false
```

Proxy object:

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    {
      ipAddress: '123.123.2.42',
      port: 8080,
      login: 'superLogin',
      password: 'superPassword'
    },
    'www.example.com',
    { inBody: '<h1>Example Domain</h1>', notInBody: '<h1>404</h1>' }
  );
})();

// return true or false
```

Proxy object, another format, instead of `login` and `password`, you can write a `loginPass`, and instead of the `ipAddress` and `port` - `ipAddressPort`:

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    {
      ipAddressPort: '123.123.2.42:8080',
      loginPass: 'superLogin:superPassword'
    },
    'www.example.com',
    { inBody: '<h1>Example Domain</h1>', notInBody: '<h1>404</h1>' }
  );
})();

// return true or false
```

Proxy object, another format:

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    {
      ipAddress: '123.123.2.42',
      port: 8080,
      loginPass: 'superLogin:superPassword'
    },
    'www.example.com',
    { inBody: '<h1>Example Domain</h1>', notInBody: '<h1>404</h1>' }
  );
})();

// return true or false
```

Of course, you can use without a password and login:

```js
const proxySimpleTest = require('proxy-simple-test');

(async () => {
  await proxySimpleTest(
    {
      ipAddress: '123.123.2.42',
      port: 8080
    },
    'www.example.com',
    { inBody: '<h1>Example Domain</h1>', notInBody: '<h1>404</h1>' }
  );
})();

// return true or false
```
