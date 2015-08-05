# Description #

An extension. Allows to bind the "this" to an object's method.<br>

<b>Current version:</b> v0.1a<br>

<b>Size:</b> 116 bytes (minified and gzipped code), 0.7 KB (uncompressed code)<br>
<br>
<br>
<b>On this page:</b>
<br>
<br>
<br>
<h1>Usage</h1>

<h2>Proxys and binding methods to events</h2>

You can bind methods to events while preserving 'this' (ie. when the method is executed, 'this' will still refer to the object, not to the event's source element). This functionality uses <a href='http://api.jquery.com/jQuery.proxy/'>jQuery.proxy()</a>.<br>
<br>
<pre><code>button = $( "#button" );<br>
<br>
// The following 2 lines give the exact same result:<br>
button.click( obj._proxy( "methodName" ) );  // You can use either this<br>
button.click( obj._proxy( obj.methodName ) );  // Or that<br>
<br>
// Actually it is a shortcut for:<br>
button.click( $.proxy( obj.methodName, obj ) );<br>
</code></pre>

<ul><li><b>object.<code>_</code>proxy( methodName | method )</b> you can use either the method name or the method itself.</li></ul>

<h2>Example of use</h2>

<pre><code>Countdown = $.klass({<br>
  init: function( from ) {<br>
    this.count = from;<br>
  },<br>
  step: function( ) {<br>
    if( this.count &gt; 0 ) this.count--;<br>
  },<br>
  hasEnded: function( ) {<br>
    return this.count === 0;<br>
  }<br>
});<br>
<br>
counter = new Countdown( 10 );  // The countdown starts at 10.<br>
button.click( counter._proxy( "step" ) );  // Each click decrements the counter.<br>
<br>
counter.hasEnded();  // =&gt; false<br>
<br>
// Now we simulate 20 clicks on the button:<br>
for ( var i = 0; i &lt; 20; i++ ) button.click();<br>
<br>
counter.hasEnded();  // =&gt; true<br>
</code></pre>


<h1>Changelog</h1>

<i>Sizes are for gzipped minified code, and then for uncompressed code.</i>

<b>v0.1a</b> (29 dec. 2012) [Sizes: 116 B, 0.7 KB]<br>
<ul><li>First commit