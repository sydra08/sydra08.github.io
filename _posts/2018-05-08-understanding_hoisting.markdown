---
layout: post
title:      "Understanding Hoisting"
date:       2018-05-08 22:31:59 +0000
permalink:  understanding_hoisting
---


TL;DR to avoid issues caused by hoisting always declare your functions and variables at the top of their scope and use `const` and `let` to declare variables instead of `var`. 

So what _is_ hoisting exactly? 

To better understand this we should take a step back and better understand the JavaScript engine. When the code is run in the browser the JS engine makes two separate passes at your code: Compilation Phase and Execution Phase. During the Compilation Phase it allocates memory and sets up identifiers for variables and functions. Then during the Execution Phase it assigns the values to the variables and invokes the functions. Hoisting is when variable and function declarations are moved to the top of the current scope to be evaluated and the assignments stay where they are (which occurs during the Compilation Phase). 

Exploring the example below should give us a better understanding. 

```
function listCharacters() {
  var superhero = "Thor";
  return `${superhero} and ${villain}`;
  var villain = "Loki";
}
```
When we execute `listCharacters()` we get `'Thor and undefined'`. If we “translate” the above to more closely match what the JS interpreter is doing we get the following: 

```
function listCharacters () {
var superhero;
var villain;

superhero = “Thor”;
return `${superhero} and ${villain}`;
villain = “Loki”;
}

listCharacters();
//=> 'Thor and undefined'
```

As you can see, the variable declarations are being “hoisted” to the top of the code but not the assignments. By line 4 in the code, the variables have been initialized with their identifiers, but have a value of `undefined`. Since the `return` occurs after the first variable (`superhero`) has been reassigned, it is the only one that is no longer `undefined`.

There are two things you can do in your code to help prevent hoisting issues: 

1) Always declare your functions and variables at the top of their scope. In doing so, you reduce the risk of having issues with `undefined` (which is also not always the most helpful when debugging). In the example above, moving both variable declarations above the `return` would give you `'Thor and Loki'` instead of `'Thor and undefined'`. 
```
function listCharacters() {
  var superhero = "Thor";
  var villain = "Loki";
  
  return `${superhero} and ${villain}`;
}

listCharacters();
//=>  'Thor and Loki'
```

2) Use `const` and `let` to declare your variables instead of `var`. There are three ways to declare variables: `const`, `let`, and `var`. All three still get hoisted, but with `let` and `const`, JavaScript doesn’t allow variables to be referenced before they have been initialized. Using the example from before, when we replace `var` with `let` the following occurs: 

```
function listCharacters() {
  let superhero = "Thor";
  return `${superhero} and ${villain}`;
  let villain = "Loki";
}

listCharacters();
// ReferenceError: villain is not defined
```
Instead of `undefined` we got a `ReferenceError`. This is because `villain` was not initialized until after the `return` and therefore could not be referenced. So while using `let` and `const` don’t solve all of your hoisting problems, getting and an error instead of `undefined`, is generally more useful when debugging. In order to fix the error, put the variable declarations above the `return` statement. 

```
function listCharacters() {
  let superhero = "Thor";
  let villain = "Loki";
  return `${superhero} and ${villain}`;
}

listCharacters();
// ‘Thor and Loki’
```

