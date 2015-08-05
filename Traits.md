# Description #

An extension. Allows very flexible traits support.<br>
What are traits ? See: <a href='http://en.wikipedia.org/wiki/Trait_%28computer_programming%29'>Trait</a> and <a href='http://stackoverflow.com/questions/925609/mixins-vs-traits'>Mixins vs. Traits</a>

<b>Current version:</b> v0.1a<br>

<b>Size:</b> 606 bytes (minified and gzipped code), 3.9 KB (uncompressed code)<br>
<br>
The code is not commented yet.<br>
<br>
<br>
<b>On this page:</b>
<br>
<br>
<br>
<h1>Usage</h1>

<h2>Simple traits</h2>

<pre><code>Draggable = $.trait({  // Create a new trait.<br>
  drag: function( ) {<br>
    return "I can be dragged.";<br>
  }<br>
});<br>
<br>
Resizeable = $.trait({<br>
  resize: function( ) {<br>
    return "I can be resized.";<br>
  }<br>
});<br>
<br>
Square = $.klass({<br>
  init: function( ) { },<br>
  _traits: [ Draggable, Resizeable ]  // Lists the traits to be added to the class<br>
});<br>
<br>
s1 = new Square();<br>
s1.drag();  // =&gt; "I can be dragged."<br>
s1.resize();  // =&gt; "I can be resized."<br>
s1._traits;  // =&gt; undefined<br>
<br>
// You cannot use instanceof for traits, but you can use the following:<br>
!! s1._hasTrait( Draggable );  // =&gt; true  "!!" coerces the value to a boolean<br>
s1._hasTrait( Draggable );  // =&gt; Square  ie: Square is the closest class that implements that trait<br>
<br>
methods = {<br>
  func: function() { }<br>
};<br>
UnusedTrait = $.trait( methods );<br>
notrait = new ( $.klass( methods ) )();<br>
!! notrait._hasTrait( UnusedTrait );  // =&gt; false<br>
notrait._hasTrait( UnusedTrait );  // =&gt; undefined<br>
</code></pre>

<ul><li><b><code>_</code>traits</b> is an array of the traits. Note: the field "<code>_</code>traits" will not be added to the klass.<br>
</li><li><b><code>_</code>hasTrait( trait )</b> searches recursively the parent classes until it find a klass that implements the trait, and return that klass or <code>undefined</code> if none is found.</li></ul>


<h2>Creating traits and name conflicts</h2>

Traits functions combination follows the principle of <b>explicit conflict resolution</b>, meaning that the conflicts won't be resolved by the program (as opposed to mixin's behavior of implicit conflict resolution). Here is how it works:<br>
<br>
<b>Name conflict rule:</b> If 2 or more traits have functions with the same name, you must define a function in the klass with the same name, otherwise an error is thrown. That function can easily call either Trait function if needed.<br>
<br>
<pre><code>SizeAnimation = $.trait({<br>
  animate: function( finalSize ) {<br>
    return "Animating size to " + finalSize + ".";<br>
  }<br>
});<br>
<br>
ColorAnimation = $.trait({<br>
  animate: function( finalColor ) {<br>
    return "Animating color to " + finalColor + ".";<br>
  }<br>
});<br>
<br>
Square = $.klass({<br>
  animate: function( finalSize, finalColor) {  // Redefining the "animate" function resolve the conflict.<br>
    // Call each trait methods:<br>
    return this._trait( arguments, SizeAnimation, finalSize ) + " "<br>
      + this._trait( arguments, ColorAnimation, finalColor );<br>
  },<br>
  animateColor: function( finalColor) {<br>
    // Call the method "animate" of the trait ColorAnimation:<br>
    return this._trait( "animate", arguments, ColorAnimation, finalColor );<br>
  },<br>
  _traits: [ SizeAnimation, ColorAnimation ]  // Name conflict: both have an "animate" function<br>
});<br>
<br>
square = new Square();<br>
square.animate( "large", "red" );  // =&gt; "Animating size to large. Animating color to red."<br>
square.animateColor( "yellow" );  // =&gt; "Animating color to yellow."<br>
<br>
// The following throws an exception because the name conflict is not resolved:<br>
Conflicting = $.klass({<br>
  _traits: [ SizeAnimation, ColorAnimation ]<br>
});<br>
</code></pre>

<ul><li><b>this.<code>_</code>trait( <code>[</code> methodName,<code>]</code> arguments, Trait, args... )</b> calls a trait method.<br>
</li><li><b>methodName</b> is an optional string containing the name of another trait method.<br>
</li><li>'<b>arguments</b>' must be the first argument (or the second if you call another trait method). Ugly but lightest way to get the right method called.</li></ul>

<b>Remember:</b> don't use in a trait a function that is already used as a method in a klass.<br>
Traits automatically wraps functions such that they can be used by different klasses. But klass doesn't wrap functions. In conclusion: never reuse in a klass a function used by a trait and vice versa.<br>
<br>
<br>
<h1>Changelog</h1>

<i>Sizes are for gzipped minified code, and then for uncompressed code.</i>

<b>v0.1a</b> (29 dec. 2012) [Sizes: 606 B, 3.9 KB]<br>
<ul><li>First version.