# JavaScript Introduction



## JavaScript defines **6 types**

<!-- slide-column 60 -->

```js
var aNumber = 3.12;
var aBoolean = true;
var aString = "HEIG-VD";
var anObject = {
  aProperty: null
};

// t is true for all of these:
var t;
t = typeof aNumber === "number";
t = typeof aBoolean === "boolean";
t = typeof aString === "string";
t = typeof anObject === "object";
t = typeof anObject.aProperty === "object";
t = typeof anObject.foo === "undefined";
```

<!-- slide-column 40 -->

They are:

* number
* boolean
* string
* object
* undefined
* null

`null` is a type, but `typeof null === object`



## JavaScript is a **dynamic** language

```js
var myVariable = "aString";
typeof myVariable; // "string"

myVariable = 3.12;
typeof myVariable; // "number"

myVariable = true;
typeof myVariable; // "boolean"

myVariable = {
  aProperty: "aValue"
};
typeof myVariable; // "object"
```

* When you declare a **variable**, you don't specify a type.
* The type can **change** over time.



## There are **2 scopes** for variables: the (evil) global scope and the function scope

```js
var aVariableInGlobalScope;

function myFunction() {
*  var aVariableInFunctionScope;
*  anotherVariableInGlobalScope;
}

function myFunction2() {
  for (i = 0; i < 10; i++) {
    // i is in global scope!
  }
  for (var j = 0; j < 10; j++) {
    // j is in function scope!
  }
}
```

* A variable declared within a function is **not accessible** outside this function.
* Unless using **strict mode**, it is not mandatory to declare variables (beware of typos...)
* Two scripts loaded from the same HTML page share the same global scope (beware of **naming conflicts**...).
* There is no **block scope**.



## JavaScript supports **first-class functions**

```js
// Named function
function multiplyByTwo(n) {
  return n * 2;
}

multiplyByTwo(3); // 6

// Stored function
var hello = function(name) {
  console.log("Hello " + name + "!");
};

hello("World"); // "Hello World!"

// Function argument
function applyToArray(array, func) {
  for (int i = 0; i < array.length; i++) {
    array[i] = func(array[i]);
  }
}

var a = [ 1, 2, 3 ];
applyToArray(a, multiplyByTwo);
console.log(a); // [ 2, 4, 6 ]
```

* New functions can be defined at **runtime**.
* Functions can be **stored** in data structures.
* Functions can be passed as **arguments** to other functions.



## Objects are **dynamic bags** of properties

```js
// Let's create an object
var person = {
  firstName: 'olivier',
  lastName: 'liechti'
};

// We can dynamically add properties
person.gender = 'male';
person['zip'] = 1446;

// And delete them
delete person.zip;

for (var key in person) {
  console.log(key + " : " + person[key]);
}
```

* There are different ways to **access properties** of an object.
* JavaScript is **dynamic**: it is possible to **add** and **remove** properties to an object at any time.
* Every object has a different list of properties (**no class**).



## Array are **objects**

```js
var fruits = ["apple", "pear"];

fruits.push("banana");
typeof fruits; // "object"

for (var i = 0; i < fruits.length; i++) {
  console.log("fruits[" + i + "] = " + fruits[i]);
}

var transformedFruits = fruits.map( function(fruit) {
  return fruit.toUpperCase();
});

transformedFruits.forEach( function(fruit) {
  console.log(fruit);
});
```



## The language has no support for **classes**

There are 3 ways to create objects.

```js
// Create an object with a literal
var person = {
  firstName: "olivier",
  lastName: "liechti"
};

// Create an object with a prototype
var child = Object.create(person);

// Create an object with a constructor
var child = new Person("olivier", "liechti");
```

* **class** is a reserved word in JavaScript, but it is not used in the current version of the language (reserved for the future).
* A **constructor** is a function like any other (uppercase is a coding convention).
* It is the use of the **new** keyword that triggers the object creation process.



## Every object **inherits** from a **prototype** object

<div class='mermaid'>
  graph TB;
    A["<strong>child</strong><br>firstName: 'Luke'"]-->|prototype|B;
    B["<strong>father</strong><br>firstName: 'Anakin'<br>lastName: 'Skywalker'"]-->|prototype|C;
    C["<strong>Object.prototype</strong>"];
</div>

```js
// Prints "Skywalker" on the console
console.log(child.lastName);
```

* Every object inherits from a prototype object. It **inherits and can override** its properties, including its methods.
* Objects created with object literals inherit from **Object.prototype**.
* When you access the property of an object, JavaScript **looks up the prototype chain** until it finds an ancestor that has a value for this property.