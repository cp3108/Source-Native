# Potential ways to achieve goals stated in the timeline

All these methods involve liberal alteration of students' code. Potential pitfalls include producing sensible error messages due to the extra function calls made and the displaying of functions (a student writing a simple function could be transpiled into a more intimidating-looking one, and saving the original copy to display to students is essential).

## Builtins

Store all builtins in a global constant named `BUILTINS`. e.g.

```javascript

const BUILTINS = {
  is_null: testee => testee === null,
  is_string: testee => typeof testee === string,
  //...
};

```

There should be other helper functions to import other libraries as well, such as curve, sound and rune.

Then, before the execution of code, all keys in `BUILTINS` will be looped through and `const <key> = BUILTINS["<key>"];` will be prepended. e.g.

```javascript
// Student's code
is_null(null);
```

```javascript
// Transpiled code
const is_null = BUILTINS["is_null"];
const is_string = BUILTINS["is_string"];
//...
// Student's code
is_null(null);
```

## Globals

Store all studentglobal variables in student's code in a global constant named `STUDENT_GLOBALS`. 

Before execution of the "main" code, `STUDENT_GLOBALS` will be reset.

Code will be appended to the end of the student's code to store the declared studentglobals.

e.g.

```javascript
//Student's main code
const PI = 3;
let sum = 0;
sum = sum + 1;
```

```javascript
//Transpiled code
//reset STUDENT_GLOBALS
// student's main code
const PI = 3;
let sum = 0;
sum = sum + 1;
//end of student's main code
//save declared studentglobals
STUDENT_GLOBALS["PI"] = {kind: "const", value: PI};
STUDENT_GLOBALS["sum"] = {kind: "let", value: sum};
```

Before exectution of REPL code (so previously declared studentglobals have to be accessible), all keys `STUDENT_GLOBALS` will be looped through and `<kind> <key> = STUDENT_GLOBALS["<key>"];` will be prepended. For all variables (those declared with `let`), `STUDENT_GLOBALS["<key>"] = <key>;` will be appended to student's code to update the variable's value.

Assuming the previous "main" code has been executed already, `PI` and `sum` will have already been saved.

So for the following code in the REPL:

```javascript
// student's repl code
sum = sum + PI;
```

```javascript
// transpiled code
const PI = STUDENT_GLOBALS["PI"];
let sum = STUDENT_GLOBALS["sum"];
// student's repl code
sum = sum + PI;
// end of student's code
STUDENT_GLOBALS["sum"] = sum;
// PI does not need to be copied back since it's constant.
```

## Strict types

In JavaScript, most operators can be used with all types, sometimes producing garbage results. In Source, strict types for operators are enforced. All operators will be transpiled into functions. There will be a global constant named OPERATORS, like: 

```javascript
const OPERATORS = {
  "+": (left, right) => {
    if (validOperands) {
      return left + right;
    } else {
      throw new Error();
    }
  }
};
```

Then student's code will be transpiled from
```javascript
1 + 2;
1 + "string";
```
into
```javascript
OPERATORS["+"](1, 2);
OPERATORS["+"](1, "string");
```

## Proper tail calls
Unfortunately (for this project, not in general), many browsers make an active decision to not support proper tail calls as specified in the ES2015 specifications. One discussion [here](https://github.com/kangax/compat-table/issues/819). 

This is required for Source though, so here's an idea: Assume that a function `enableProperTailCalls` exists. (More will be written on how it works at a later date, I believe it deserves it own page.) There is a working prototype [here](https://repl.it/@daryltan/tail-recursion).

Then, all functions that students have declared, 

```javascript
function sumTo(n, sum) {
  return n === 0 ? sum : sumTo(n - 1, sum + n);
}

const factorial = (n, total) {
  return n === 0 ? total : factorial(n - 1, n * total);
}

const squared = map(list(1, 2, 3), x => x * x);
```

will be transpiled into

```javascript
const sumTo = enableProperTailCalls((n, sum) => {
  return n === 0 ? sum : sumTo(n - 1, sum + n);
});

const factorial = enableProperTailCalls((n, total) => {
  return n === 0 ? total : factorial(n - 1, n * total);
});

const squared = map(list(1, 2, 3), enableProperTailCalls(x => x * x));
```
