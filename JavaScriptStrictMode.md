# Introduction #

The code in jquery.klass.js which accesses a super method finds that method through "arguments.callee". Thus this library wont work if run in [ECMAScript 5 Strict Mode](https://developer.mozilla.org/en/JavaScript/Strict_mode). (For now, this limitation only concerns special use of the JavaScript engine) However, it will always be able to work along with other scripts that are running in Strict Mode.

But in the futur a replacement solution exists. It's called: named function expressions.

# Named function expressions #

When all common browser become compatible with [named function expressions](https://developer.mozilla.org/en/JavaScript/Reference/Operators/Special/function#section_6), then we could stop using the "arguments" variable.
Here is an example of what would become possible:

```
$.klass(Parent, {
  methodName: function func(arg) {  // This is a named function expression, the name is "func".
    this._super(arguments, arg);	// <- Instead of this
    this._super(func, arg);	// <- We could do this
    func._super(arg);	// <- Or even that !
  }
});
```

Pretty cool isn't it ?

Here are some of the benefits:
  * $.klass uses "arguments.callee" to access the properties of a function but:
    * "arguments.callee" is [discouraged](https://developer.mozilla.org/en/JavaScript/Reference/Functions_and_function_scope/arguments/callee).
    * "arguments.callee" is forbidden in [ECMAScript 5 Strict Mode](https://developer.mozilla.org/en/JavaScript/Strict_mode) (and might be deprecated in a future release of JavaScript).
  * in some cases the presence of "arguments" defeats some optimizations that JavaScript engines could apply (like function inlining).
  * "arguments" cannot be shortenned by JavaScript minifiers, but "func" can be.
  * "arguments" is ugly.
But for now this is only a dream. Because as explained in Kangax's [Named function expressions demystified](http://kangax.github.com/nfe/), Internet Explorer still has some serious bugs with it (at least up to IE 8 apparently).