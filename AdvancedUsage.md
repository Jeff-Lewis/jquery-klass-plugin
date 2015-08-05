## Add/remove methods ##

```
Message = $.klass({
  init: function( message ) {
    this.message = message;
  },
  getMessage: function( ) {
    return this.message;
  }
});

ImportantMessage = $.klass( Message, {
  init: function( message ) {
    this._super( arguments, message + " !" );
  }
});

error = new ImportantMessage( "An error occurred" );
error.getMessage();  // => "An error occurred !"
// Ok, that message doesn't look so important.


// So we make the new function that will become our new method:
getMessage2 = function( ) {
  return this._super( arguments ).toUpperCase();
};
getMessage2._klass = { klass: ImportantMessage, name: "getMessage" };

// Now we add the new method to the class ImportantMessage:
ImportantMessage.prototype.getMessage = getMessage2;


error.getMessage();  // => "AN ERROR OCCURRED !"
// Now it looks much better !



// To completly remove a method, simply do:
delete ImportantMessage.prototype.getMessage;
```