# Description #

An extension. Allows private fields.

**Current version:** v0.1a<br>

<b>Size:</b> 692 bytes (minified and gzipped code), 4.3 KB (uncompressed code)<br>
<br>
The code is not commented yet.<br>
<br>
<br>
<b>On this page:</b>
<br>
<br>
<br>
<h1>Usage</h1>

<h2>Private properties</h2>

<pre><code>Image = $.klass({<br>
  _private: {<br>
    init: function( privateProps, name ) {<br>
      privateProps.fileName = name + ".png";<br>
    },<br>
    getSrc: function( privateProps, withPath ) {<br>
      return ( withPath ? "images/" : "" ) + privateProps.fileName;<br>
    },<br>
    tryToGetValue: function( privateProps ) {<br>
      return privateProps.value;<br>
    }<br>
  }<br>
});<br>
<br>
Icon = $.klass( Image, {<br>
   init: function( type ) {<br>
    this._super( arguments, type );<br>
  },<br>
  _private: {<br>
    setValue: function( privateProps, value ) {<br>
      privateProps.value = value;<br>
    },<br>
    getValue: function( privateProps ) {<br>
      return privateProps.value;<br>
    },<br>
    tryToGetFileName: function( privateProps ) {<br>
      return privateProps.fileName;<br>
    },<br>
    askNicelyForFileName: function( privateProps ) {<br>
      return this._super( "getSrc", arguments, false );<br>
    }<br>
  }<br>
});<br>
<br>
icon = new Icon( "info" );<br>
icon.getSrc( true );  // =&gt; "images/info.png"<br>
icon.setValue( 42 );<br>
icon.getValue( );  // =&gt; 42<br>
<br>
// Here we see clearly that methods from one class don't have access to private properties of another class:<br>
icon.tryToGetValue( );  // =&gt; undefined<br>
icon.tryToGetFileName( );  // =&gt; undefined<br>
<br>
// Any other feature works as expected. For example let's try super method call:<br>
icon.askNicelyForFileName( );  // =&gt; "info.png"<br>
</code></pre>

<ul><li><b><code>_</code>private</b> is  where you define functions that have access to the private properties. Note: the field "<code>_</code>private" will not be added to the klass.<br>
</li><li><b>privateProps</b> the first argument is an object containing the private properties. You can use another name if you prefer.</li></ul>

<b>Security note</b>: it is still possible for other code to access the private property. The inheritance model is flexible. So for instance, it is possible to add new methods to your klass that have acccess to the private properties. But there is an easy way to make it more secure if you need to. See <a href='AdvancedUsage.md'>advanced usages</a>.<br>
<br>
<br>
<h2>Secure private properties</h2>

Because klass inheritance is purely prototype based, it is easy to add/remove any method to any class. That means that it is possible to add any method that will have access to your private properties. You can prevent this by securing the class when it is created. That means that only the methods defined at the class creation will be able to receive the private properties. Any attempt to use another function will fail with a security exception.<br>
<br>
How does it works ? Here is the idea: we maintain a list of allowed methods for each class, and we perform additional security check before each method call with private properties for that class. Of course that means that method calls on secured class will be a bit slower.<br>
<br>
In this feature, the weak point is the class creation. Once the class is created, no more malicious code can get access to the private properties. Before the class is created, any code could alter the class creation process to get access to the private properties of any future created class.<br>
<br>
<br>
Here is how to secure a class:<br>
<br>
<pre><code>Account = $.klass({<br>
  init: function( privateProps, name ) {<br>
    this.name = name;<br>
  },<br>
  _private: {<br>
    _secure: true,  // Set this property to true to secure the class.<br>
    setPassword: function( privateProps, password ) {<br>
      privateProps.password = password;<br>
    },<br>
    login: function( privateProps ) {<br>
      // send the name and the password to the server<br>
    }<br>
  }<br>
});<br>
</code></pre>

<ul><li><b><code>_</code>secure</b>: if this property exists in <code>_</code>private and is set to true, then the private properties will be accessible only by the methods defined in <code>_</code>private at class creation.</li></ul>


<h1>Changelog</h1>

<i>Sizes are for gzipped minified code, and then for uncompressed code.</i>

<b>v0.1a</b> (29 dec. 2012) [Sizes: 692 B, 4.3 KB]<br>
<ul><li>First version.