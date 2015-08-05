# Description #

Full class implementation in JavaScript for jQuery 1.4+

  * **Highly extensible**. Everything is exposed. Add new functionalities as easily as with jQuery.
  * Supports **multiple levels of inheritance** (Some JavaScript libraries only support one)
  * Fully **prototype based** inheritance, allowing the use of the **'instanceof' operator**
  * **Implicit constructors**
  * Supports calling the **super method** or **any method** of the super class
  * Easily use **class variables** (also known as static variables)
  * **Faster** than "closure" based classes
  * **Light syntax**
  * Modify methods at run time, thanks to **dynamic super method** calls
  * **Saves memory** compared to "closure" based classes (1 or 1000 instances all share the same methods)
  * Short **jQuery syntax** like "$.klass", "init"
  * Your code can be **minified** without limitation. Klass doesn't rely on the string representation of functions (See: [Closure Compiler Limitations](http://code.google.com/intl/fr/closure/compiler/docs/limitations.html))

A few extensions are available to add the functionnality you need.
  * **private properties**: a private variables solution for prototype based inheritance
  * **traits**: add the same methods to multiple classes (much like mixins)
  * **proxy**:
  * **namespace**:

There are 3 projects:
  * jquery-klass-plugin: a complete rewrite. Cleaner code, more extensible.
  * [jquery-tinyklass-plugin](http://code.google.com/p/jquery-tinyklass-plugin/): the original project. Focus on small code size.
  * [jquery-klass-qunit](http://code.google.com/p/jquery-klass-qunit/): unit tests for the 2 above projects

See the [Overview](Overview.md) to get started.

Or directly go to the documentation:
  * [Klass](Klass.md)
  * [Private properties](PrivateProperties.md)
  * [Traits](Traits.md)
  * [Proxy](Proxy.md)
  * [Namespace](Namespace.md)