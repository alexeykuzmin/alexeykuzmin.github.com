---
layout: post
title: "jsonpointer.js"
date: 2013-05-02 01:01
comments: true
categories:
 - javascript
 - english
---

Although JSON&#160;Pointer did not become very popular since
its [appearance in 2011](http://tools.ietf.org/html/draft-pbryan-zyp-json-pointer-00),
it's still alive and even has evolved into [proposed standard](http://tools.ietf.org/html/rfc6901).

<!-- more -->


## What JSON Pointer is

It's hard to explain it better than specification does:

> JSON Pointer defines a string syntax for identifying a specific value
> within a JavaScript Object Notation (JSON) document.

It is similar to [JSON&#160;Path](http://goessner.net/articles/JsonPath/),
but unlike the latter JSON&#160;Pointer extracts only *existing&#160;values* from document
and does not do any filtering or merging.

There is copy of evaluation examples from spec. Given the JSON document
{% codeblock lang:json %}
{
    "foo": ["bar", "baz"],
    "": 0,
    "a/b": 1,
    "c%d": 2,
    "e^f": 3,
    "g|h": 4,
    "i\\j": 5,
    "k\"l": 6,
    " ": 7,
    "m~n": 8
}
{% endcodeblock %}

The following strings evaluate to the accompanying values:
{% codeblock lang:bash %}
""       ->  whole document
"/foo"   ->  ["bar", "baz"]
"/foo/0" ->  "bar"
"/"      ->  0
"/a~1b"  ->  1
"/c%d"   ->  2
"/e^f"   ->  3
"/g|h"   ->  4
"/i\\j"  ->  5
"/k\"l"  ->  6
"/ "     ->  7
"/m~0n"  ->  8
{% endcodeblock %}


## Existing implementations

There is a bunch of JSON&#160;Pointer implementations in&#160;different languages:
[Erlang](https://github.com/janl/erl-jsonpointer),
[Go](https://github.com/dustin/go-jsonpointer),
[Perl](https://github.com/zigorou/perl-json-pointer),
[PHP](https://github.com/raphaelstolt/php-jsonpointer),
[Python](https://github.com/stefankoegl/python-json-pointer),
and there are
[two](https://github.com/janl/node-jsonpointer)
[implementations](https://github.com/manuelstofer/json-pointer)
in JavaScript.

Both of JavaScript implementations work only in Node.js environment
and&#160;implement something bigger than specification defines.
They support methods not only for retrieving values
but also for modifying JSON documents and other stuff.


## jsonpointer.js

Since there was no client-side implementation of JSON&#160;Pointer in JavaScript
I have created my own one – [jsonpointer.js](https://github.com/alexeykuzmin/jsonpointer.js).
It implements only retrieving of values defined by specification,
works and all modern browsers and can be used as standalone script or [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) module.
Also it works in Node.js environment and [is available](https://npmjs.org/package/jsonpointer.js) via [npm](https://npmjs.org).

Here are several examples of its usage:
{% codeblock lang:html %}
<script src="/path/to/jsonpointer.js" type="text/javascript"></script>
<script>

// XXX: Target must be a string!
var targetJSON = JSON.stringify({
  foo: {
    bar: 'foobar'
  },
  baz: [true, false]
});

jsonpointer.get(targetJSON, '/foo');  // {bar: 'foobar'}
jsonpointer.get(targetJSON, '/baz/test');  // undefined

// Partial application.
var evaluate = jsonpointer.get(targetJSON);
evaluate('/foo/bar');  // 'foobar'
evaluate('/baz');  // [true, false]

</script>
{% endcodeblock %}

Detailed description of API can be found in project [README](https://github.com/alexeykuzmin/jsonpointer.js#readme).
