# Description #

The core of the library. Allows full class inheritance.

**Current version:** v0.1a<br>

<b>Size:</b> 637 bytes (minified and gzipped code), 7.7 KB (uncompressed code)<br>
<br>
<br>
<b>On this page:</b>
<br>
<br>
<br>
<h1>Usage</h1>

<h2>A simple class</h2>

<pre><code>Counter = $.klass({<br>
  init: function( start ) {<br>
    this.count = start;<br>
  },<br>
  isLimitReached: function( limit ) {<br>
    return this.count &gt;= limit;<br>
  },<br>
  increment: function( ) {<br>
    this.count++;<br>
    return this.count;<br>
  },<br>
  toString: function() {<br>
    return "Count: " + this.count + " !";<br>
  }<br>
});<br>
</code></pre>

<ul><li><b>init</b> will be the constructor of the class. If you don't provide one, klass will make an implicit constructor for you.</li></ul>

<b>Note:</b> If you want 2 classes to use the same function, you must either write the function twice, use a wrapper function or use traits. Never use the same exact function (ie. 2 references to the same function) for 2 different classes !<br>
<br>
<br>
<h2>Instantiating and using a class</h2>

<pre><code>counter = new Counter( 0 );<br>
<br>
counter.increment( );  // =&gt; 1<br>
counter.count;  // =&gt; 1<br>
counter.isLimitReached( 0 );  // =&gt; true<br>
counter.isLimitReached( 100 );  // =&gt; false<br>
<br>
// And you have:<br>
counter instanceof Counter;  // =&gt; true<br>
counter.init === Counter;  // =&gt; true<br>
</code></pre>

<ul><li><b>init</b> is the class and the constructor of the object. Use it to get the class of an object.</li></ul>

<h2>Subclass with inheritance</h2>

<pre><code>LimitedCounter = $.klass( Counter, {  // Inherit from Counter<br>
  init: function( start, limit ) {<br>
    this._super( arguments, start );  // Call the super method (which is "init", the constructor of Counter)<br>
    this.limit = limit;<br>
  },<br>
  isLimitReached: function( ) {<br>
    // Call the super method (ie: the method "isLimitReached" from the super-class Counter):<br>
    return this._super( arguments, this.limit ) ? "Limit reached !" : "Nope.";<br>
  },<br>
  increment: function( ) {<br>
    // Call the method "isLimitReached" from the super-class Counter:<br>
    if ( !this._super( "isLimitReached", arguments, this.limit ) ) {<br>
    	 // Call the super method (ie: the method "increment" from the super-class Counter):<br>
    	this._super( arguments );<br>
    }<br>
    return this.toString();<br>
  }<br>
});<br>
<br>
limitedCounter1 = new LimitedCounter( 0, 0 );  // Limit already reached<br>
limitedCounter1.isLimitReached( );  // =&gt; "Limit reached !"<br>
limitedCounter1.increment( );  // =&gt; "Count: 0 !"<br>
<br>
limitedCounter2 = new LimitedCounter( 10, 100 );  // Limit already reached<br>
limitedCounter2.isLimitReached( );  // =&gt; "Nope."<br>
limitedCounter2.increment( );  // =&gt; "Count: 11 !"<br>
<br>
// And you have:<br>
limitedCounter1 instanceof LimitedCounter;  // =&gt; true<br>
limitedCounter1 instanceof Counter;  // =&gt; true<br>
LimitedCounter._super === Counter;  // =&gt; true<br>
Counter._super;  // =&gt; undefined<br>
</code></pre>

<ul><li><b>class.<code>_</code>super</b> to get the super-class.<br>
</li><li><b>this.<code>_</code>super( <code>[</code> methodName,<code>]</code> arguments, args... )</b> calls the super method.<br>
<ul><li><b>methodName</b> is an optional string containing the name of another super-method.<br>
</li><li>'<b>arguments</b>' must be the first argument (or the second if you call another super method). Ugly but lightest way to get the superclass method calling to work.</li></ul></li></ul>


<h2>Class variables</h2>

Also known as static variables.<br>
<br>
<pre><code>MyKlass = $.klass({<br>
  init: function() {<br>
    MyKlass.nbInstances++;  // modify the class variable.<br>
  },<br>
  getNbInstances: function( ) {<br>
    return MyKlass.nbInstances;  // access the class variable<br>
  },<br>
  _klass: {  // Everything defined here (not only functions) will become a class variable (or class variable).<br>
    nbInstances: 0,<br>
    resetNbInstances: function() {<br>
      this.nbInstances = 0;  // Here 'this' refers to MyKlass.<br>
    }<br>
  }<br>
});<br>
<br>
n1 = new MyKlass();<br>
n1.getNbInstances();  // =&gt; 1<br>
n1._klass, undefined;  // =&gt; undefined<br>
<br>
new MyKlass(); new MyKlass(); new MyKlass();<br>
n2 = new MyKlass();<br>
n1.getNbInstances();  // =&gt; 5<br>
n2.getNbInstances();  // =&gt; 5<br>
MyKlass.resetNbInstances();<br>
n2.getNbInstances();  // =&gt; 0<br>
</code></pre>

<ul><li><b><code>_</code>klass</b> all its properties will become class variables. Note: the field "<code>_</code>klass" will not be added to the klass.</li></ul>


<h1>Changelog</h1>

<i>Sizes are for gzipped minified code, and then for uncompressed code.</i>

<b>v0.1a</b> (29 dec. 2012) [Sizes: 637 B, 7.7 KB]<br>
<ul><li>First version.