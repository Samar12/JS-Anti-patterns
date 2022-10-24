# Anti-patterns

Imagine that you’re a soldier in the middle of war & you are supposed to kill your enemy with guns and army-related things, but you choose to kill your enemies with a scissor, at the end you may kill your enemy and achieve the goal, and your reason to join the war but you’re doing it in a wrong way! You made this job harder, messier, and more unstructured.
It’s almost the same in the software engineering world and you may do this thing every day without really being informed about it and I’ll help you find some of these anti-patterns in JS.
While it’s quite important to be aware of [good] design patterns, it can be equally important to understand anti-patterns. In 1955, Koenig once presented his thought on anti-patterns

> Anti-patterns are bad solutions to a particular problem that resulted in a bad situation

let’s go over some of the JavaScript anti-patterns you should avoid: 


## 1.	Modifying the Object class prototype
Don’t modify objects you do not own to write a maintainable JavaScript


### Bad

```javascript
function Person (first, last, age, gender, intersets) {

    // property and method definitions
    this.name = {
      'first': first,
      'last': last
    };

    this.age = age;
    this.gender = gender;
}

let person = new Person('Bob', 'Smith', 32, 'male', ['music', 'skiing']);
```

You could modify Bob Smith to anyone you like to (change his name and gender etc…)

```javascript
person1.age = 50;
person1.gender = 'male';

console.log(person1);
/* Person {
     name: {first: 'Bob', last: 'Smith'},
     age = 50,
     gender = 'male
} */
```

But please DO NOT do that! 

The same method was being used in other places with its original intended usages but not behaving as expected because it was overridden


### Solution
1. DO NOT modify the Object prototype directly.
2. Create a new Object using ```Object.assign```
```javascript
let person2 = Object.assign({}, new Person('SJ', 'Steven', 32, 'male', ['music', 'skiing']));

console.log(person2);
// {name: {first: 'Bob', last: 'Smith'}, age: 32, interests: ['music', 'skiing']}
```



## 2. Passing a string to `setTimeout` or `setInterval`
Passing a string to setTimeout or setInterval is evil because it is run in the global scope and has performance issues


### Problem Identify
First of all, ```setTimeout()``` method calls a function or evaluates an expression after a specified number of milliseconds


The following code won’t work while it prints out HelloWorld immediately because it has the following ERROR ```Callback must be a function```

```javascript
  setTimeout(console.log('HelloWorld'), 1000);
```

Similarly, passing a string to setTimeout or setInterval is anti-pattern which cause the following ERROR ```Callback must be a function```

```javascript
  setTimeout('doSomething(someVar)', 1000);

  function doSomething(someVar) {
    console.log(someVar);
  }
```

### Solution
Use function as a callback instead of string:


1) classical function callback
```javascript
  setTimeout(function() {
    console.log(someVar)
  }, 1000);
```
2) arrow function callback

```javascript
  setTimeout(() => console.log(someVar), 1000);
```

## 3. JavaScript loops

I can say for sure the most common anti-pattern we made in our codes is related to looping-related things and the wrong pick of ```for…of, forEach and the map```.


Let's start with an awful JS map usage:     
```javascript
[1, 2, 3].map(i => console.log('Logged to console ${i} times'));

//log
Logged to console 1 times
Logged to console 2 times
Logged to console 3 times
```
See, we have the correct output and behavior we expected from this loop! so what’s wrong?


JavaScript map is an array method that is designed to loop through an array and return an array as the output and result to you! according to the MDN description, `The map() method creates a new array populated with the results of calling a provided function on every element in the calling array.` so it means that when you want to have an array as an output then you can use this method as well like the below example:

```javascript 
const myArr = [1, 4, 9, 16];
const myMap = myArr.map(x => x * 2);

console.log(myMap);
// expected output: Array [2, 8, 18, 32]
```

So what is the pattern for logging into the console 5 times? When you are thinking about this and you remembered a method or HOF you memorized and used before in your code just please simply search and read the documentation from the official website and see what is the purpose of this method and how can you use it in the best way you can, for example, we can simply log to console with a for…of in JavaScript.

```javascript
for (let step = 0; step < 3; step++) {
    // runs 3 times
    console.log('Logged to console ${step + 1} times');

    Logged to console 1 times
    Logged to console 2 times
    Logged to console 3 times
}
```

## 4. Do not use the == to check for equality
Before checking or executing arithmetic operations, JavaScript first executes an implicit type of coercion. This enables JavaScript to operate with different types of variables, but implicit type coercion may complicate the code as a whole and sometimes buries type errors caused in arithmetic operations. In order to use the ```==``` operator properly, the author and the reader of the program must fully understand the underlying concepts of type coercion.

### Bad
```javascript
const emptyArr = new Array();
const emptyObj = new Object();

const arr = new Array(1, 2, 3, 4, 5);
const obj = new Object();
obj.prop1 = 'val1';
obj.prop2 = 'val2';

const arr2 = new Array(5);
arr2.length // 5
```
### Good
```javascript
const emptyArr = [];
const emptyObj = {};

const arr = [1, 2, 3, 4, 5];
const obj = {
  prop1: 'val1',
  prop2: 'val2'
};
```

## 5. Naming convention
Your naming convention needs to be very clear and descriptive, whenever you see a variable or a function name you should be able to guess what to expect as a value coming from there, have a look into the following examples.

### Bad

```javascript
// This is a user name
let n = "Jone Due"

// This is for a year
let yyyy = 2022

// This function should duplicate a number
function dNum(n) {
  return n * 2
}

// This function is checking if 2 numbers are equal
function equal(x, y) {
  return x === y
}
```


### Good
```javascript
// This is a user name
let userName = "Jone Due"

// This is for a year
let currentYear = 2022

// This function should duplicate a number
function doubleNumber(num) {
  return num * 2
}

// This function is checking if 2 numbers are equal
function isEqual(x, y) {
  return x === y
}
```

## 6. Improper use of Truthy and Falsy values
In JavaScript, `Falsy` and `Truthy` values are a bit different than in other tools, and by `Falsy` means equal to `false`, and `Truthy` means when we check it is equal to `true`, we have about 5 different values which are Falsy in JavaScript which might be wise if you check for each one in the proper way instead of checking in general if they are falsy, check below for best practices.
### Bad

```javascript
function isTruthy(y) {
  return !y ? false : true
}

let x
isTruthy(x) // false

x = 0
isTruthy(x) // false

x = ""
isTruthy(x) // false

x = null
isTruthy(x) // false

x = NaN
isTruthy(x) // falsen x === y
}
```


### Good
(A comment in front of each value tells you how to check that value)
```javascript
function isTruthy(y) {
  return !y ? false : true
}

let x
isTruthy(x) // This is okay to check undefined

x = 0
isTruthy(x) // check -> x !== 0 || x > 0

x = ""
isTruthy(x) // check -> x !== '' || x?.length > 0

x = null
isTruthy(x) // check -> x ?? true : false

x = NaN
isTruthy(x) // check -> isNaN(x)
```

## Conclusion
In this article, you should have learned what are design patterns and what are anti-patterns, and you learned 6 different examples of anti-patterns/bad practices to avoid in your next JavaScript project

> Note that we can’t point out all of the anti-patterns of JS here because it’s really based on you and how bad and an un-researching guy you are. You’ll have anti-patterns increasing in your codebase if you’re not researching well enough.😊

