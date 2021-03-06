[![logo][logo-url]][npm-url]

# bloomrun
[![git][git-badge]][git-url]
[![npm][npm-badge]][npm-url]
[![travis][travis-badge]][travis-url]
[![coveralls][coveralls-badge]][coveralls-url]
[![david][david-badge]][david-url]

A js pattern matcher based on bloom filters, inspired by [patrun](http://npm.im/patrun).
But different: 10x faster, and with results that can be returned in __insertion order__ or __depth order__.

* [Install](#install)
* [Example](#example)
* [API](#api)
* [Acknowledgements](#acknowledgements)
* [License](#license)

<a name="install"></a>
## Install
To install bloomrun, simply use npm:

```
npm install bloomrun --save
```

<a name="example"></a>
## Example
The example below can be found [here][example] and ran using `node example.js`. It
demonstrates how to use bloomrun for pattern matching with a payload.

```js
'use strict'

var bloomrun = require('bloomrun')()

bloomrun.add({say: 'hello' }, 'Hello World!')
bloomrun.add({say: 'goodbye'}, function () {
  console.log('Goodbye World!')
})

var hello = bloomrun.lookup({say: 'hello'})
console.log(hello)

var goodbye = bloomrun.lookup({say: 'goodbye'})
goodbye()
```

<a name="api"></a>
## API

  * <a href="#constructor"><code><b>bloomrun()</b></code></a>
  * <a href="#add"><code>instance.<b>add()</b></code></a>
  * <a href="#remove"><code>instance.<b>remove()</b></code></a>
  * <a href="#lookup"><code>instance.<b>lookup()</b></code></a>
  * <a href="#iterator"><code>instance.<b>iterator()</b></code></a>
  * <a href="#list"><code>instance.<b>list()</b></code></a>

-------------------------------------------------------
<a name="constructor"></a>
### bloomrun([opts])

Creates a new instance of Bloomrun.

Options are:

* `indexing`: it can be either `insertion` (default) or `depth`;
  if set to `insertion`, it will try to match entries in insertion order;
  if set to `depth`, it will try to match entries with the most
  properties first.

-------------------------------------------------------
<a name="add"></a>
### instance.add(pattern [,payload])

Adds a pattern to the Bloomrun instance. You can also provide an alternative
payload to return instead of the pattern itself. This allows pattern based
retrieval of objects. If no payload is provided the pattern itself will be
returned.

-------------------------------------------------------

<a name="remove"></a>
### instance.remove(pattern [,payload])

Removes a pattern from the Bloomrun instance. Filters are rebuilt after each
removal which may mean the same pattern is matched by another filter. In cases
where two patterns differ only by payload, the supplied payload can be used to
determine the correct match. If no payload is supplied any matched pattern will
be removed regardless of it's own payload.

-------------------------------------------------------

<a name="lookup"></a>
### instance.lookup(obj [, opts])

Looks up the first entry that matches the given obj. A match happens
when all properties of the added pattern matches with the one in the
passed obj. If a payload was provided it will be returned instead of
the pattern.

Options:
 * `patterns: true`, if you want to retrieve only patterns, not
   payloads

-------------------------------------------------------
<a name="iterator"></a>
### instance.iterator(obj [, opts])

Returns an iterator, which is an object with a `next` method. `next`
will return the next pattern that matches the object or `null` if there
are no more.
If `obj` is null, all patterns/payload will be returned.

Options:
 * `patterns: true`, if you want to retrieve only patterns, not
   payloads

-------------------------------------------------------
<a name="list"></a>
### instance.list(obj [, opts])

Returns all patterns that matches the object. If a payload was provided
this will be returned instead of the pattern.
If `obj` is null, all patterns/payload will be returned.

Options:
 * `patterns: true`, if you want to retrieve only patterns, not
   payloads

## Acknowledgements

This library is heavily inspired by Richard Rodger's
[patrun](http://npm.im/patrun) and [seneca](http://npm.im/seneca).
Also, It would not be possible without
[bloomfilter](https://www.npmjs.com/package/bloomfilter).

The bloomrun logo was created, with thanks, by [Dean McDonnell](https:/github.com/mcdonnelldean)

## License

Copyright Matteo Collina 2015-2016, Licensed under [MIT][].

[MIT]: ./LICENSE
[example]: ./example.js

[travis-badge]: https://img.shields.io/travis/mcollina/bloomrun.svg?style=flat-square
[travis-url]: https://travis-ci.org/mcollina/bloomrun
[git-badge]: https://img.shields.io/github/release/mcollina/bloomrun.svg?style=flat-square
[git-url]: https://github.com/mcollina/bloomrun/releases
[npm-badge]: https://img.shields.io/npm/v/bloomrun.svg?style=flat-square
[npm-url]: https://npmjs.org/package/bloomrun
[logo-url]: https://rawgit.com/mcollina/bloomrun/master/assets/bloomrun.svg
[coveralls-badge]: https://img.shields.io/coveralls/mcollina/bloomrun/badge.svg?branch=master&service=github&style=flat-square
[coveralls-url]: https://coveralls.io/github/mcollina/bloomrun?branch=master
[david-badge]: https://david-dm.org/mcollina/bloomrun.svg?style=flat-square
[david-url]: https://david-dm.org/mcollina/bloomrun
