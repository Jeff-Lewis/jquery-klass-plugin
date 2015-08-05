**This is an old page I wrote before implementing private properties. Actually, this is what gave me the idea about private properties. For now I keep this page as it is. I will update it when I have time.**


---


Private variables/properties are a common concern in JavaScript class systems.

In klass, as with common prototype based inheritance, you cannot have private properties or private variables.

**- Why ?**

Because:
  1. there is no private properties in JavaScript. There is no access modifier for properties in JavaScript.
  1. klass is not designed as to let you use private variables in your objects.

**- But I have seen others...**

Yes there are workarounds. They use a JavaScript mechanism called "closure": if a variable is defined within a method it will only be accessible from code inside that method. It needs all object methods to be defined by a function at object instantiation.

**- Why don't use closures for private variables in klass ?**

Because there are too many disadvantages:
  * methods added through traits and sub-classes wouldn't be able to access those private variables.
  * no more memory saving: methods could not be shared by all instances, but each object would have it's own copy of all the methods.
  * inheritance could not be fully prototype based anymore.
  * it would add a possibly large overhead at each object instantiation (instead of only a small one at class definition).
  * changing a class at runtime would not be reflected in all its objects (for those who want to add methods dynamically).

Basically you must choose either closure based classes or prototype based classes. And klass is a pure prototype based class system.

**Then what do you do when you need private variables ?**

Like most people, I prefix the variable name with an underscore: "`_`". This is a widely used programming convention. It means that code from outside should never read or write that variable directly. Being a convention, it can not prevent others from doing it. But at least they know that if they do it, it may not work as they expect if the class implementation is modified at any time (even at runtime).

**What about secret variables like passwords ?**

Actually I believe that in most cases an evil code could always access your secret value either before you receive it (or using the same method you use to receive it), or when you send it to somewhere else (like the server or another script).

**So there are really no way ?**

There are other class systems available on the internet. Even some that try to allow both prototype based and closure based class hierarchy. Life is a matter of compromise. Find the one that better suits your needs

**Links**
  * [JavaScript Inheritance via Prototypes and Closures](http://www.ruzee.com/blog/2008/12/javascript-inheritance-via-prototypes-and-closures): with examples of each kind.
  * Stackoverflow: [How to “properly” create a custom object in JavaScript?](http://stackoverflow.com/questions/1595611/how-to-properly-create-a-custom-object-in-javascript)