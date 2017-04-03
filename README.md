<!--
Location: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Callbacks & Iterator Methods

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

Using callback functions is an effective way to write declarative, functional JavaScript. JavaScript was built to deal with asynchronous events; callbacks help appropriately time and coordinate our program.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Pass a function as a callback to another function
- Use iterator methods with callbacks to build more declarative loop structures
- Recognize the best iterator method for a particular use case
- Build iterator methods from scratch


### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Write and call functions in JavaScript
- Explain what a higher order function is
- Use a `for` loop



<!--### The Call Stack-->

<!--We say that a function **takes in arguments** and **returns** something to us. You can imagine JavaScript control flow as a person talking on the phone with your program. When you call a function, it's like JS puts the main program on hold and contacts the function. If another function is called, JS puts the first function on hold to contact the new one. When the call to that function finishes, JS returns to the previous call.-->

<!--To keep track of the functions JavaScript has on hold, it uses a **call stack**. As JavaScript calls a new function, it pushes the function and its important variables onto the call stack. When a function returns, it pops that function off of the top of the call stack. The return value is "substituted in" for the function call after the function returns.-->

<!--```js-->
<!--function add(a, b){-->
<!--  return a + b;-->
<!--}-->
<!--function main(x, y){-->
<!--  var z = add(x, y);-->
<!--  console.log(z);-->
<!--  return z;-->
<!--}-->
<!--main(3, 2);-->
<!--```-->
<!--Here's what the call stack would look like for the `main(3,2)` call above:-->

<!--<img src="images/simple_call_stack.png">-->


<!--#### Recursive Example-->

<!--```js-->
<!--function factorial(n){-->
<!--    if (n <= 1) {-->
<!--        return 1;-->
<!--    } else {-->
<!--        return n * factorial(n-1);-->
<!--    }-->
<!--}-->
<!--```-->

<!--Here's what the call stack would look like if we call `factorial(3)` given the code above. (The function name is shortened to `fact` for space.)-->

<!--<img src="images/fact_call_stack.png">-->


<!--A function that calls itself is called a **recursive** function.-->



### Callbacks: Review

![callback](http://i.giphy.com/l0MYM5ZAXGvK1KSPK.gif)

A **callback** is a function that is passed into another function as an argument and then used. A function that can take in a callback as an argument is known as a higher order function.

Let's review with an example.


```js

function masterMathFunction(a, b, callback) {
    console.log("the inputs are: " + a + " and " + b);
    callback(a,b);
}

function multiplyMe(n,m) {
    console.log("Product: " + n*m);
}

function raiseMe(n,m) {
    console.log("Power: "+ Math.pow(n,m));
}

masterMathFunction(2,3,raiseMe);
masterMathFunction(2,3,multiplyMe);

```

1. What is the higher order function here?

2. What function(s) may be used as callbacks for the higher order function?

3. What is another possible callback function?

Callbacks allow us to queue up the execution of a function until after some other code completes. They allow for asynchronous behavior, even though JavaScript is a single-threaded language. They also let us customize behaviors inside libraries. 

<!--In order to accomplish this, callbacks don't just use the regular call stack. They also involve a structure called a callback queue (or line).  When the callback is ready to be run, it's added to the queue. Callbacks waiting in the queue are run whenever the stack is empty.-->

See this awesome video of [a talk by Philip Roberts](https://www.youtube.com/watch?v=8aGhZQkoFbQ) on how JavaScript works.

#### Check for Understanding

Let's walk through another example of code that uses callbacks:

```javascript
var element = document.querySelector("body");
var counter = 0;
element.addEventListener("click", countClicks);

function countClicks(event){
  counter += 1;
  console.log("clicked " + counter + " times.");
}
```

Let's run this in the console.


#### Anonymous Functions: Review

Often, if a callback will only be used with one higher order function, the callback function definition is written inside the higher order function call.


```js
var element = document.querySelector("body");
var counter = 0;
element.addEventListener("click", function(event){
  counter += 1;
  console.log("clicked " + counter + " times.");
});
```

In these cases, the callback often won't be given a name.  A function without a name is called an **anonymous function**.

Here's an ES6 version using an arrow function:


```js
let element = document.querySelector("body");
let counter = 0;
element.addEventListener("click", (event) => {
  counter += 1;
  console.log("clicked " + counter + " times.");
});
```

#### Independent Practice: `sort`

JavaScript's built-in `sort` method for arrays sorts number values (by the first digits' place) and String items as strings, alphabetically.

```js
var arr = [1, 2, 125, 500];
arr.sort();
// returns [1, 125, 2, 500]
```

Checking the [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort), you should notice there is an optional `compareFunction` parameter that can change the sort order rules. 

Use JavaScript's `sort` function to sort the following objects by price, from lowest to highest:

```js
var items = [
  { name: "trail mix", price: 3.50 },
  { name: "first aid kit", price: 20.00 },
  { name: "water bottle", price: 12.00 },
  { name: "flashlight", price: 8.00 },
  { name: "gps unit", price: 93.00 }
];
```

<details>
  <summary>Hint: how to start</summary>
  You'll need to write a custom `compareFunction` and pass it into the `sort` method. Follow the structure of the custom `compareFunction` from the documentation.
</details>

<details>
  <summary>Answer: the compare function</summary>
  ```js
  function compareByPrice(item1, item2){
    if (item1.price < item2.price) {
      return -1;
    }
    if (item1.price > item2.price) {
      return 1;
    }
    // items must have equal price
    return 0;
  }
  ```
</details>

<details>
  <summary>Answer: calling `sort` with customized function</summary>
  ```js
  items.sort(compareByPrice);
  ```
</details>



### Iterator Methods

**Iteration** basically means looping.

![looping](http://i.giphy.com/GB3XCL0cnIUxi.gif)

```javascript
var potatoes = ["Yukon Gold", "Russet", "Yellow Finn", "Kestrel"];
for(var i=0; i < potatoes.length; i++){
    console.log(potatoes[i] + "!")
}
```

**Iterator methods** create more declarative abstractions for common uses of loops.

```js
var potatoes = ["Yukon Gold", "Russet", "Yellow Finn", "Kestrel"];
potatoes.forEach(function(element){
  console.log(element + "!")
});
```

We can combine our knowledge of **callbacks** & **iteration** to write better, more *declarative* code.

*We can also illustrate a lot of these with the physical example of a pillbox(array) with items at each index(jelly beans!).*

Be thinking about how we could extend this metaphor with each method we discuss here.

### [`forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/ForEach)


The `forEach()` method performs whatever callback function you pass into it on each element.

```javascript
var fruits = ["Apple", "Banana", "Cherry", "Durian", "Elderberry",
"Fig", "Guava", "Huckleberry", "Ice plant", "Jackfruit"];

fruits.forEach(function (value, index) {
    console.log(index + ". " + value);
});

// 0. Apple
// 1. Banana
// 2. Cherry
// 3. Durian
// 4. Elderberry
// 5. Fig
// 6. Guava
// 7. Huckleberry
// 8. Ice plant
// 9. Jackfruit
//=> ["Apple", "Banana", "Cherry", "Durian", "Elderberry",
//    "Fig", "Guava", "Huckleberry", "Ice plant", "Jackfruit"];
```

### [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Map)
Similar to `forEach()`, `map()` traverses an array. This method, however performs the callback function you pass into it on each element and then outputs the results inside a new array.

Often we want to do more than just perform an action, like console.log(), on every loop.  When we actually want to modify/manipulate our array, `map` is the go-to!

####Example: Double every number

```js
var numbers = [1, 4, 9];
var doubles = numbers.map(function doubler(num) {
  return num * 2;
});
// doubles is now [2, 8, 18]. numbers is still [1, 4, 9]
```

####Example: Pluralize all the fruit names

```javascript
pluralized_fruits = fruits.map(function pluralize(element) {

  // if word ends in 'y', remove 'y' and add 'ies' to the end
    var lastLetter = element[element.length -1];
     if (lastLetter === 'y') {
      element = element.slice(0,element.length-1) + 'ie';
  }

    return element + 's';
});

fruits // ORIGINAL ARRAY IS UNCHANGED!
//=> ["Apple", "Banana", "Cherry", "Durian", "Elderberry",
//    "Fig", "Guava", "Huckleberry", "Ice plant", "Jackfruit"];

pluralized_fruits // MAP OUTPUT
//=> [ "Apples", "Bananas", "Cherries", "Durians", "Elderberries",
//    "Figs", "Guavas", "Huckleberries", "Ice plants", "Jackfruits"  ]
```

####Example: Square each number

```javascript
var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

numbers.map(function square(element) {
  return Math.pow(element, 2);
});
//=> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

#### Check for Understanding: Etsy Merchant

<details>
<summary>
Elaine the Etsy Merchant thinks her prices are scaring off customers. Subtracting one penny from every price might help!  Use `map` to subtract 1 cent from each of the prices on this receipt's list:

```javascript
var prices = [3.00, 4.00, 10.00, 2.25, 3.01];
// create a new array with the reduced prices...
```
</summary>

```javascript
var prices = [3.00,4.00,10.00,2.25,3.01];
var reducedPrices = prices.map(function(price) {
  return price - 0.01;
});
```

</details>


### [`filter`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Filter)
With the `filter()`, method you can create a new array filled with elements that pass certain criteria that you designate.  This is great for creating a new filtered list of movies that have a certain genre, fruits that start with vowels, even numbers, and so on.  
  *It's important to remember that a filter method on an array requires a `boolean` return value for the callback function.*

#### Example: Return a list of fruits that start with vowels

```javascript
var fruits = ["Apple", "Banana", "Cherry", "Elderberry",
"Fig", "Guava", "Ice plant", "Jackfruit"];
var vowels = ["A", "E", "I", "O", "U"];
function vowelFruit(fruit) {
  var result = vowels.indexOf(fruit[0]) >= 0; // indexOf returns -1 if not found
  // console.log("result for " + fruit + " is " + result);
  return result;
}
var vowelFruits = fruits.filter(vowelFruit);
console.log(vowelFruits);
// ["Apple", "Elderberry", "Ice plant"]
```

Or alternatively:

```javascript
var vowels = ["A", "E", "I", "O", "U"];

var vowelFruits = fruits.filter(function vowelFruit(fruit) {
  return vowels.indexOf(fruit[0]) >= 0; // indexOf returns -1 if not found
});
console.log(vowelFruits);
// ["Apple", "Elderberry", "Ice plant"]

```

#### Example: Find all the even numbers that are greater than 5

```javascript
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

even = numbers.filter(function filterEvens(num) {
  var isEven = num%2==0;
  var greaterThanFive = num > 5;
  return isEven && greaterThanFive;
});
//=> [6, 8, 10]
```

#### Check for Understanding: Birthdays

<details>
<summary>
Is there an interesting trend in birthdays?  Do people tend to be born more on even-numbered dates or odd-numbered dates? If so, what's the ratio? In class, let's take a quick poll of the days of the month people were born on. This is a great chance to do some serious science!

```javascript
var exampleBdays = [1, 1, 2, 3, 3, 3, 5, 5, 6, 6, 8, 8, 10, 10, 12, 12, 13, 13, 15, 17, 17, 18, 20, 20, 26, 31];
// gather an array of all the even birthdays...
```
</summary>

```javascript
var exampleBdays = [1, 1, 2, 3, 3, 3, 5, 5, 6, 6, 8, 8, 10, 10, 12, 12, 13, 13, 15, 17, 17, 18, 20, 20, 26, 31];
var birthDateEvens = exampleBdays.filter(function(birthday) {
  return birthday % 2 === 0 ? birthday : false;
});
```
</details>

### [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
The `reduce()` method is designed to create one single value that is the result of an action performed on all elements in an array.  It essentially 'reduces' the values of an array into one single value.

#### Example: Find the sum of all of the numbers in an array

```javascript
var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

var sum = numbers.reduce(function add(previous, current) {
  return current + previous;
});

//=> 55

```

> In the above examples, notice how the first time the callback is called it receives `element[0]` and `element[1]`.

There is another way to call this function and specify an initial starting value.

```javascript
var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

var sum = numbers.reduce(function add(previous, current) {
  return current + previous;
}, 100);

//=> 155
```

In the above example, the first time the callback is called it receives `100` and `1`.

> **Note**: We set the starting value to `100` by passing in an optional extra argument to `reduce`.

#### Check for Understanding: Test Scores

<details>
<summary>
Roberto has been tracking his test scores over the semester. Use `reduce` to help you find his average test score.

```javascript
var scores = [85, 78, 92, 90, 98];
```
</summary>

```javascript
var scores = [85, 78, 92, 90, 98];
var total = scores.reduce(function(previous, current) {
  return previous + current;
});
var average = total/(scores.length);
```
</details>

#### Discussion: `forEach`

So how does `forEach` work?

Let's think about `forEach` again. What's happening behind the scenes?

* What are our inputs?  
* What is our output?  
* What happens on each loop?  
* What does the callback function do?  
* What gets passed into our callback function? That is, what are its inputs/parameters?  
* Where does the callback come from?

Let's check:

```javascript
function print(item) {
  console.log(item);
}

[0, 100, 200, 300].forEach(function(number) {
  print(number);
});
```

<details>
<summary>Given the above, how would you build a function that mimics `forEach` yourself? Call it `myForEach`.</summary>

```javascript
function myForEach(collection, callback) {
  for(var i = 0; i < collection.length; i++) {
    callback(collection[i]);
  }
}
// the below should have the same result as the above
myForEach([0, 100, 200, 300], print)
```
</details>

### Closing Thoughts

- Callbacks are a very common pattern in JavaScript.  
- Iterator methods help us write more conventional, declarative code. Loops (`for` and `while`) are more imperative and offer greater flexibility (configuration).
