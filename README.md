# Design patterns with Javascript
I made this because I wanted a document / summary with understandable examples of the design patterns in JavaScript. Other than that, I found that some of these patterns are difficult to understand in Javascript mainly due to the flexibility of the language, so I made examples of each one to better understand. So here is my document / summary on design patterns with Javascript. I hope it can be useful for someone.

**Note:** For better understanding you should have a medium level in JavaScript.

**To keep in mind:** 
- ***Arrow functions doesn't have "this context".***
- Design patterns are flexible!

<pre>
<code>
TO DO LIST
[] Finish Behavior patterns

[X] "javascript" for all code instead of "diff"
[X] delete # and replace - and + by (deleted) and (added) for all code

[X] Table of content Structural patterns AND test it
[] Table of content Behavior patterns AND test it
</code>
</pre>


### There are three types of patterns
#### * Creational: Those who help us to create objects.
#### * Structural:  These, help us to communicate different stuctures.
#### * Behavior: Those who help us to disengaged our code for to be easiest to understand.

## Table of Contents  
- **[Creational patterns.](#creational)**
  -  [Constructor.](#constr)
  -  [Constructor with prototypes.](#constrproto)
  -  [Module.](#modul)
  -  [Revealing module.](#revmodule)
  -  [Prototype.](#prototy)

- **[Structural patterns.](#structural)**
  -  [Mixin.](#mixi)
  -  [Decorator.](#decorato)
  -  [Facade.](#facad)
  -  [Adaptator.](#adaptado)

- **[Behavior patterns.](#behavior)**
  - [Observer.](#observe)
  - [Mediator.](#mediato)
  - [Command.](#comman)
  - [Chain of responsability.](#chain)

<br/>

<a name="creational"/>

## Creational patterns:

<a name="constr"/>

### Constructor:
Here we use the <code>new</code> keyword to create a new object by instantiating a class (this facility or sintactic sugar comes with ES6).

Example.js (**with ES6+**):
```javascript
class MyClass {
  // Look, the Constructor is here in the below line!
  constructor(propertyValue) { 
    this.property = propertyValue;
    this.property2 = 'hello';
    this.method = () => {
       // I'm the method
    }
  }
}

// We're using the "new" keyword in the next line
const myNewInstance = new MyClass('prop'); 
console.log(myNewInstance); // MyClass {property: 'prop', property2: 'hello', method: [function]}
```
<br/>

<a name="constrproto"/>

### Constructor with prototypes:
It is similar to the previous one but look that it changes where we defined the method. Be careful with this use because if you instance the class multiple times and change the method in one of these instances, you will change that method for all other instances.

Example.js (**with ES6+**):
```javascript
class MyClass {
  constructor(propertyValue) {
    this.property = propertyValue;
    this.property2 = 'hello';
-(deleted)   
  }
  // We're using arrow functions here (comes with ES6 as well)
+(added) method = () => { 
+(added)     // I'm the method
+(added)   }
}

const myNewInstance = new MyClass('property'); 
console.log(myNewInstance); // MyClass {property: 'prop', property2: 'hello', method: [function]}
```
**Remember that almost everything in JavaScript are objects or instances of it, (the only that it isn't objects are the primitive values or types), let's check this statement out with an example from ES5:**

Example2.js (**with ES5**):
```javascript
// We're adding a new method to the Object that will show the Object in the console.
Object.prototype.log = function () {
  console.log(this)
}

var myObj = { Mykey: 'myValue' };
// The above line is syntactic sugar for:
var myObj = new Object();
Object.defineProperty(myObj, 'myKey', {value: 'myValue'});

myObj.log() // {myKey: 'myValue'};

// We're adding a new method to the String object that will show the String in the console as a warning
String.prototype.showWarn = function () { 
// Look! String is an instance of Object, we're accessing to the Prototype (you might already have seen it before as __proto__ in the browser), so yes, almost everything in JavaScript are objects!
  console.warn(this);
}

var sayHi = 'Hola';
sayHi.showWarn() // (warning) hola
```
<br/>

<a name="modul"/>

### Module:
It is based on the objects literals in JavaScript. When we define a literal object in JavaScript that has methods and properties, we are defining a module.

Example.js (**with ES6+**):
```javascript
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

<a name="revmodule"/>


### Revealing module:
Unlike the **Module** design pattern, here, we use closures to define methods and properties either private or public.

Example.js (**with ES6+**):
```javascript
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

<a name="prototy"/>

### Prototype:
It's different to ***Constructor with prototypes pattern***. It's based on that we can take a defined object and based on that object we can create another prototypes to other objects (it's how the OOP inheritance is handle with JavaScript ES5), again, thank you ES6 for made the life easier!

Example.js (**with ES5**):
```javascript
var dog = {
  breed: 'rottweiler',
  // We're not using arrow functions in the line above because these doesn't have "this context" and because we're using ES5 in this example
  bark: function () {
    consolel.log('Guau, Im a' + this.breed);
  }
}

console.log(dog) // {breed: 'rotweiler', bark: [function]}

var mySecondDog = Object.create(dog);
// If I'll try to show mySecondDog it logs an empty object because  the properties and methods are in the prototype, I mean in dog
console.log(mySecondDog) // {}

mySecondDog.bark(); // 'Guau, I'm a rottweiler'
mySecondDog.breed = 'bullterrier';
mySecondDog.bark() // 'Guau, I'm a bullterrier'

// If we replace a property or a method, it'll be owned only by the object that it changed it, in the example, only by mySecondDog, the prototype won't be modified
console.log(mySecondDog) // {breed: 'bullterrier'}
```
<br/>

<a name="structural"/>

## Structural patterns:

<a name="mixi"/>

### Mixin:
This pattern will help us to add more functionality to the prototype of our classes with no need to alter the code inside the class. Therefore, all other new instances of that class will contain the original class and the extended functionalities.

Example.js (**with ES6**):
```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

//The next one is the object with new methods for the User class
let mixin = {
  sayHi() {
  console.log(`Hi ${this.name}!`);
  },
  sayBye() {
  console.log(`Good bye ${this.name}!`);
  }
}

// We increase the prototype
Object.assign(User.prototype, mixin);

const myUser = new User('Toothless');
console.log(myUser) // {name: "Toothless", __proto__: {sayHi: [function], sayBye: [function], constructor: Class User}}
myUser.sayHi(); // Hi Toothless

const myAnotherUser = new User('Astrid');
console.log(myAnotherUser) // {name: "Astrid", __proto__: {sayHi: [function], sayBye: [function], constructor: Class User}}
myAnotherUser.sayHi(); // Hi Astrid
```

<br/>

<a name="decorato"/>

### Decorator: 
It's similar to Mixin pattern, but the main difference is that the Decorator pattern helps us to ONLY add more functionality to a specific instance of the class, not to all instances as Mixin does. Therefore, only a specific instance will change and all other new instances of that class will contain ONLY the original class without the extended functionalities. 

I'll use the same example as Mixin so you can see the differences.

Example.js (**with ES6**):
```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const myUser = new User('Toothless');
myUser.addLastname = function (lastname) {
    return `${this.name} ${lastname}`;
}

console.log(myUser) // {name: "Toothless", addLastname: [function], __proto__: constructor: class User}
myUser.addLastname('Nightfury') // Toothless Nightfury

const myAnotherUser = new User('Astrid');
console.log(myAnotherUser); // {name: "Astrid", __proto__: constructor: class User}
```

<br/>

<a name="facad"/>

### Facade: 
It is used when we want to simplify the call to a function. For example calling another function called <code>get</code> to make a XMLHttpRequest call.

Tip: XMLHttpRequest is just for example, you can use browser API fetch or axios, both have already applied this pattern for you ease.

Example.js (**with ES6**):
```javascript
import https from 'https';

// we're using the get arrow function to enclose all the XMLHttpRequest boilerplate
const get = (url) => new Promise((resolve, reject), => {
  const completeFormat = url.split('/');
  const host = completeFormat.shift();
  
  const options = {
    hostname: host,
    path: `/${completeFormat.join('/')}`,
    method: 'GET'
  };
  
  const req = https.request(options, res => {
    res.setEncoding('utf8');
    let body = '';
    res.on('data', data => {
      body += data;  
    });
    
    res.on('end', data => {
      const parsed = JSON.parse(body);
      resolve(parsed);
    });
    
    req.on('error', (err) => {
       reject(err);
    });
    
    req.end();
  })
});

// And here we just call the get arrow function instead of all the XMLHttpRequest boilerplate each time
const asyncFunctionToCallGet = async () => {
  const result = await get('jsonplaceholder.typicode.com/users');
  console.log(result); // [{resolved promise}]
};
```

<br/>

<a name="adaptado"/>

### Adaptator:
It's useful when we're using a class, method or library it's starting to give us problems and we want to update it. For update it we'll develop an adapter.

Example.js (**with ES6**):
```javascript
// this is our fist (and a little outdated) version of the class
class Api {
  constructor() {
    this.operations = function (url, opts, verb) {
      switch (verb) {
        case 'get':
          // return browser api fetch...
        case 'post':
          // return browser api fetch...
        default:
          return
      }
    };
  }
}

// Our new version of the class
class Api2 {
  constructor() {
    this.get = function (url, opts) {
      // return axios.get...
    };
    this.post = function (url, opts) {
      // return axios.post
    };
  }
}

// And here we applied the adapter pattern, that if you notice the structure is similar to the first one (Api class) but under the hood it's using the second one or the new version (Api2 class)
class ApiAdapter {
  constructor () {
    const api2 = new Api2();
    
    this.operations = function (url, opts, verb) {
      switch (verb) {
        case 'get':
          return api2.get(url, opts);
        case 'post':
          return api2.post(url, opts);
        default:
          return
      }
    };
  }
}

// for example, we had been using the first version (Api class) with the fetch browser api
const api = new Api();
api.operations('www.amazon.com', { x: 1 }, 'get');

// however we create the new version to use axios because this use the fetch browser api and XMLHttpRequest directly for other browsers that require it that way, so since we create the new version, our team only uses the new way of calling http requests.
const api2 = new Api2();
api2.get('www.amazon.com', { x: 1 });

// but what happen with all the legacy code that it's using the first version for make http calls? Have we to replace it all?... The answer is no because we create an adapter for that, so we only have to replace the call, not the implementation.
-(deleted) const api = new Api();
-(deleted) api.operations('www.amazon.com', { x: 1 }, 'get');
+(added) const adapter = new ApiAdapter();
+(added) adapter.operations('www.amazon.com', { x: 1 }, 'get');
```

<br/>

<a name="behavior"/>

## Behavior patterns:

<br/>

<a name="observe"/>

### Observer:
The observer pattern consists of two actors, the Observable(or Subject) who publish something and the Observer who subscribe to the Observable changes. They'll be able to interact with each other. So, if the Observable has a change, the Observer who is subscribed will be able to listen that changes and do something (execute a code for example) based on that change. I wrote a good example here: https://josedanielhq.medium.com/observer-pattern-for-dummies-with-javascript-af5e653f75f6

Example.js (**with ES6**):
```javascript
// First, we define the YoutubeChannel class
class YoutubeChannel {
  observers = [];
  
  subscribe(observer) {
    this.observers.push(observer);
  }

  unsubscribe(observer) {
    const index = this.observers.findIndex(obs => {
      return obs === observer;
    });
  }

  notify(newData) {
    this.observers.forEach(observer => observer.update(newData))
  }
}

// Second, we define the YoutubeSubscriptor class that will receive the updates from YoutubeChannel.
class YoutubeSubscriptor {

  update(newData) {
    console.log(`We have received new data, that is: ${newData}`);
  }
}

// Using the classes created above
const pewDiePie = new YoutubeChannel();
const normalUser = new YoutubeSubscriptor();
pewDiePie.subscribe(normalUser);
pewDiePie.notify('I have a new video');  // We have received new data, that is: I have a new video
```

<br/>

<a name="mediato"/>

### Mediator:
It's similar to the Observer pattern, but in this case, with Observables and Observers intervented by a Mediator. This Mediator will be in charge to handle and dispatch all the events between Observers and Observables. Redux is a famous library that uses this pattern.

Example.js (**with ES6**):
```javascript
const Mediator = (() => {
  const observers = [];
  
  return {
    subscribe: (newObserver) => {
      observers.push(newObserver);
    },
    emit: (observable, infoToNotify) {
      observers.forEach((item) => console.log(`${infoToNotify} comming from ${observable}`));
    }
  };
})() // Auto-called function expression

function iWannaSubscribe() {
  // Some random code here
};

Mediator().subscribe(iWannaSubscribe);

function iWannaNotify() {
  // Another random code here
}

Mediator().emit(iWannaNotify, {hello: 'world'});

```

<br/>

<a name="comman"/>

### Command:
It give us an unified interface to execute methods without execute its directly, you will able to execute those internal methods with commands as "run", "execute", "call" and so on.

Example.js (**with ES6**):
```javascript
const stocksCommander = (() => {
  const commands = {
    buy: (stock) => {
      console.log(`Buying ${stock}`);
    },
    sell: (stock) => {
      console.log(`Selling ${stock}`);
    }
  };
  
  return {
    run: (command, stockName) => {
      if (!commands[command]) {
        console.log('non-existent command');
        return undefined;
      }
      commands[command](stockName)
    }
  };
})();

stockCommander.run('buy', AMZN) // Buying AMZN
stockCommander.run('sell', TSLA) // Selling TSLA
stocksCommander.run('idunno', 'trying to break the code') // non-existent command - undefined
```


### Chain of responsability:
It's based on we're going to encapsulate a data and we're going to add methods to this data to be able to alter the value that is contained.

<br/>

<a name="chain"/>

Example.js (**with ES6**):
```javascript
class Sum {
  constructor(v = 0) {
    this.val = v;
  }
  
  add(v) {
    this.val += v;
    return this; // This return the class Sum, so we could chain this method as we will do below
  }
}

const value = new Sum(1);
value
  .sum(1) // 2
  .sum(2) // 4
  .sum(3) // 7
  .val // 7
```
