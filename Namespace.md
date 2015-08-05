# Description #

An extension. Allows the use of namespaces (packages).<br>

<b>Current version:</b> v0.1a<br>

<b>Size:</b> 770 bytes (minified and gzipped code), 4.1 KB (uncompressed code)<br>
<br>
The code is not commented yet.<br>
<br>
<br>
<b>On this page:</b>
<br>
<br>
<br>
<h1>Usage</h1>

<h2>Namespace</h2>

Klass creation usage: <code>$.klass( [ fullKlassName, ] [ superKlass, ] { method1, ... } )</code>

Suppose we want:<br>
<ul><li>class Animal in namespace animal<br>
</li><li>class Mammal in namespace animal.mammal extending animal.Animal<br>
</li><li>class Dolphin in namespace animal.mammal extending animal.mammal.Mammal</li></ul>

<pre><code>A = $.klass( "animal.Animal", { init: function () {} } );<br>
M = $.klass( "animal.mammal.Mammal", "animal.Animal", { init: function () {} } );<br>
D = $.klass( "animal.mammal.Dolphin", "animal.mammal.Mammal", { init: function () {} } );<br>
// Namespaces are automatically created as needed.<br>
<br>
// Now you have:<br>
animal.mammal.Dolphin === D;  // =&gt; true<br>
window.animal.mammal.Dolphin === D;  // =&gt; true<br>
D._super === M;  // =&gt; true<br>
</code></pre>


<h2>KlassPath</h2>

A KlassPath object represents a named klass in a namespace. That klass may or may not have been created yet.<br>
<br>
<pre><code>// Get a klass from a klass path:<br>
$.klassPath( "animal.mammal.Dolphin" ).get() === D;  // =&gt; true<br>
$.klassPath( "animal.mammal.DoesNotExists" ).get() === undefined;  // =&gt; true<br>
<br>
// Get a KlassPath from a klass:<br>
A.klassPath === $.klassPath( "animal.Animal" );  // =&gt; true<br>
<br>
// Get a KlassPath from an object instance:<br>
dolphin = new D();<br>
dolphin.init.klassPath === D.klassPath;  // =&gt; true<br>
<br>
// Useful properties of KlassPath:<br>
D.klassPath.namespaceName === "animal.mammal";  // =&gt; true<br>
D.klassPath.klassName === "Dolphin";  // =&gt; true<br>
D.klassPath.fullKlassName === "animal.mammal.Dolphin";  // =&gt; true<br>
</code></pre>

<ul><li><b>get()</b> finds and returns the klass referred by a KlassPath.<br>
</li><li><b>namespaceName</b> the name of the namespace for the KlassPath.<br>
</li><li><b>klassName</b> the klass name of the klass referred by the KlassPath.<br>
</li><li><b>fullKlassName</b> the fully qualified klass name of the klass referred by the KlassPath. (ie. "namespaceName.klassName")</li></ul>

You can also use a KlassPath when you create a new klass to set it's super klass and/or it's klass path:<br>
<br>
<pre><code><br>
M_kp = $.klassPath( "animal.mammal.Mammal" );<br>
T_kp = $.klassPath( "animal.mammal.Tiger" );<br>
T_kp.get() === undefined;  // =&gt; true<br>
<br>
T = $.klass( T_kp, M_kp, { } );<br>
<br>
T_kp.get() === T;  // =&gt; true<br>
T._super === M;  // =&gt; true<br>
</code></pre>


<h1>Changelog</h1>

<i>Sizes are for gzipped minified code, and then for uncompressed code.</i>

<b>v0.1a</b> (29 dec. 2012) [Sizes: 770 B, 4.1 KB]<br>
<ul><li>First commit