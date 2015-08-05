# Klass FAQ #

**Why is the plugin called "klass" with a "k" instead of a "c" ?**

"class" is a reserved word in JavaScript.

**Why do you have a method named "`_`super" ? Wouldn't "super" have been better ?**

"super" is also a reserved word in JavaScript.

**Why do you prefix the methods with a "`_`" ? Like "`_`proxy", ...**

So you can use the names "proxy", ... in you classes. And also to stay consistent with "`_`super".

**Can I change the name of a method ?**

Sure. The safest way is to add an alias like this:

```
$.Class = $.klass;  // Now you can use $.Class() instead of $.klass()
$.klass.prototype.up = $.klass.prototype._super;  // Now you can use obj.up() instead of obj._super()
```

Also you can change the names in the source code if you know how to do it.

**What minifier do you use ?**

Google's [Closure Compiler](http://code.google.com/p/closure-compiler/).<br>
Also note that I add a header with informations (name, author, license and link), but I do not include it in the size of the gzipped minified codes. That is because I am only interested in actual code size. That header adds about 120 characters.<br>
<br>
<b>Can I remove the comment header ? (with name, author, license and project link)</b>

Yes, you can. I tried to make it as small as possible. Though I don't mind if you remove it completely if you need to. For example if you rearrange the code for your own needs. Otherwise, please keep at least one reference to this project somewhere near the klass code.<br>
<br>
<b>Other class implementations:</b>

<ul><li>John Resig's <a href='http://ejohn.org/blog/simple-javascript-inheritance/'>Simple JavaScript Inheritance</a>. Inspired by Prototype and Base2 (using $super).</li></ul>

<ul><li><a href='https://code.google.com/p/joose-js/'>Joose</a>. A meta object system for JavaScript.</li></ul>

<ul><li><a href='http://traitsjs.org/'>Traits</a>. No class, only traits, but very flexible.</li></ul>

<ul><li><a href='http://code.google.com/p/core-framework/'>Core-Framework</a>. Namespace, class, mixins, delegation, autoload.</li></ul>

<ul><li><a href='http://flesler.blogspot.ch/2008/06/jsclass.html'>jsClass</a>. Small code, has extensions for many functionalities.</li></ul>

<h1>From Sunny Hirai's FAQ</h1>

<i>You can find his FAQ <a href='http://code.google.com/p/jquery-klass/wiki/FAQ'>here</a>.</i>

<i>Why do you have to pass 'arguments' to call a method in the superclass when using this.<b>up()</b></i>

This is an idea from Joshua Gertzen's excellent article on "Object Oriented Super Class Method Calling with JavaScript." There are other ways to accomplish the same thing including a neat method used in the JavaScript Prototype framework.<br>
<br>
The Prototype model is cleaner to read but just as difficult to understand (in Prototype's model, you need to add a $super argument on the receiving method, in Klass you need to add an 'arguments' argument in the calling method).<br>
<br>
There are two reasons why I went for this model<br>
<br>
(a) performance. The Prototype model requires you to run a regular expression which tend to be CPU intensive<br>
<br>
(b) code size. Attaching a reference to a function results in smaller code size. Together, these two ideas matched better with jQuery fundamentals.<br>
<br>
<b>Why did you write yet another implementation of class?</b>

(a) Because there wasn't a good implementation of a class plugin for jQuery that I could find (b) I wanted to guarantee there would be at least one good class plugin for jQuery I can use for my team's development of extendable classes and (c) I really wanted to contribute in an important way to the jQuery community. I have hope that this or a derivative will make it into core.