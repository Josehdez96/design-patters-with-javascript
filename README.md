# Design patterns with Javascript
I did this because I bought a course on design patterns and I want a summary for remember all the topics. Other than that I found that some of these patterns are difficult to understand in Javascript because mainly due its flexibility. So here is my summary on design patterns with Javascript. I hope it can be useful for someelse. 

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
Here we use the <code>new</code> keyword to create a new object by instantiating a class (this facility or sintactic sugar comes with ES6)

Example.js (**with ES6+**):
```diff
class MyClass {
  constructor(propertyValue) { // Look the Constructor here!
    this.property = propertyValue;
    this.property2 = 'hello';
    this.method = () => { // We're using arrow functions here (comes with ES6 as well)
      // I'm the method
    }
  }
}

const myNewInstance = new MyClass('property'); // We're using the "new" keyword.
console.log(myNewInstance);
```
<br/>

### Constructor with prototypes:
It is similar to the previous one but look that it changes where we defined the method. Be careful with this use because if you instance the class multiple times and change the method in one of those instances, you will change that method for all other instances.

Example.js (**with ES6+**):
```diff
class MyClass {
  constructor(propertyValue) { // Look the Constructor here!
    this.property = propertyValue;
    this.property2 = 'hello';
-   
  }
+ method = () => { // We're using arrow functions here (comes with ES6 as well)
+     // I'm the method
+   }
}


const myNewInstance = new MyClass('property'); // We're using the "new" keyword.
console.log(myNewInstance);
```

