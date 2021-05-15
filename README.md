# Design patterns with Javascript
I did this because I bought a course on design patterns and I want a summary for remember all the topics. Other than that I found that some of these patterns are difficult to understand in Javascript because mainly due its flexibility. So here is my summary on design patterns with Javascript. I hope it can be useful for someelse.

**Note:** For better understanding you should have a midium level in JavaScript.

Remember: Design patterns are flexible!

To get started, we're able to use design patterns if these three rules are accomplished:
1. Meets a goal
2. Is usefull
3. It has a wide application

### There are three types of patterns
#### * Creational: Those who help us to create objects.
#### * Structural:  These help us to communicate different stuctures.
#### * Behavior: Help to disengaged our code for to be easiest to understand.


## Creational patterns:

### Constructor:
Here we use the <code>new</code> keyword to create a new object by instantiating a class (this facility or sintactic sugar comes with ES6).

Example.js (**with ES6+**):
```diff
class MyClass {
# // Look, the Constructor is here in the below line!
  constructor(propertyValue) { 
    this.property = propertyValue;
    this.property2 = 'hello';
    this.method = () => {
#      // I'm the method
    }
  }
}

# // We're using the "new" keyword in the next line
const myNewInstance = new MyClass('prop'); 
console.log(myNewInstance); // MyClass {property: 'prop', property2: 'hello', method: [function]}
```
<br/>

### Constructor with prototypes:
It is similar to the previous one but look that it changes where we defined the method. Be careful with this use because if you instance the class multiple times and change the method in one of these instances, you will change that method for all other instances.

Example.js (**with ES6+**):
```diff
class MyClass {
  constructor(propertyValue) {
    this.property = propertyValue;
    this.property2 = 'hello';
-   
  }
#  // We're using arrow functions here (comes with ES6 as well)
+ method = () => { 
+     // I'm the method
+   }
}

const myNewInstance = new MyClass('property'); 
console.log(myNewInstance); // MyClass {property: 'prop', property2: 'hello', method: [function]}
```
**Remember that almost everything in JavaScript are objects or instances of it, (the only that it isn't objects are the primitive values or types), let's check this statement out with an example from ES5:**

Example2.js (**with ES5**):
```diff
# // We're adding a new method to the Object that will show the Object in the console.
Object.prototype.log = function () {
  console.log(this)
}

var myObj = { Mykey: 'myValue' };
# // The above line is syntactic sugar for:
var myObj = new Object();
Object.defineProperty(myObj, 'myKey', {value: 'myValue'});

myObj.log() // {myKey: 'myValue'};

# // We're adding a new method to the String object that will show the String in the console as a warning
String.prototype.showWarn = function () { 
# // Look! String is an instance of Object, we're accessing to the Prototype (you might already have seen it before as __proto__ in the browser), so yes, almost everything in JavaScript are objects!
  console.warn(this);
}

var sayHi = 'Hola';
sayHi.showWarn() // (warning) hola
```
<br/>

### Module:
It is based on the objects literals in JavaScript. When we define an object literal in JavaScript which one has methods and properties we're defining a module.

Example.js (**with ES6+**):
```diff
const myModule = {
  prop: 'myProp';
  config: {
    language: 'en',
    cache: false
  },
  setConfig: (config) =< {
   myModule.config = config;
  },
  showLanguage: () => {
    console.log(myModule.config.language)
  }
}

myModule.prop // 'myProp'
myModule.showLanguage() // 'en'
```
<br/>

### Revealing module:
Unlike the **Module** design pattern, here, we use closures to define methods and properties either private or public.

Example.js (**with ES6+**):
```diff
const myRevealingModule = (() => {
  const myObj = {};
  
  function showData() { // private method
    console.log('myObj is:', myObj)
  });
  
  return { // public methods
    show: () => console.log(myObj),
    add: (key, val) => myObj[key] = val
  }
})()

myRevealingModule.add('myKey', 'myValue');
myRevealingModule.showData() // TypeError: it is not a function (because it's private)
myRevealingModule.show() // {myKey: 'myValue'}
```
<br/>

### Prototype:
It's different to ***Constructor with prototypes pattern***. It is based in that we can take a defined object and based on that object we can create another prototypes to other objects (it's how the OOP inheritance is handle with JavaScript ES5), again, thank you ES6 for made the life easier!

Example.js (**with ES5**):
```diff
var dog = {
  breed: 'rottweiler',
#  // We're not using arrow functions in the line above because these doesn't have "this context" and because we're using ES5 in this example
  bark: function () {
    consolel.log('Guau, I'm a' + this.breed);
  }
}

var mySecondDog = Object.create(dog);
mySecondDog.bark(); // 'Guau, I'm a rottweiler'
mySecondDog.breed = 'bullterrier';
mySecondDog.bark() // 'Guau, I'm a bullterrier'
```
<br/>
