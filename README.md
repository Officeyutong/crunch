Crunch [![Build Status](https://travis-ci.org/vukicevic/crunch.svg?branch=master)](https://travis-ci.org/vukicevic/crunch) [![NPM version](https://badge.fury.io/js/number-crunch.svg)](http://badge.fury.io/js/number-crunch) [![Bower version](https://badge.fury.io/bo/crunch.svg)](http://badge.fury.io/bo/crunch)
======
Crunch is an [arbitrary-precision integer arithmetic](http://en.wikipedia.org/wiki/Arbitrary-precision_arithmetic) library for JavaScript.

* [Homepage](http://crunch.secureroom.net/)
* [Examples](http://crunch.secureroom.net/examples/)
* [Tests](http://crunch.secureroom.net/tests/)

It was designed to execute arithmetic operations as quickly as possible, in particular those upon which asymmetric encryption cryptosystems such as RSA are built.

Usage
-----
Crunch can be loaded as a classic browser script

```html
<script src="crunch.js"></script>
<script>
var crunch = Crunch();
</script>
```

or in a web worker

```javascript
var crunch = new Worker("crunch.js");
```

or it can be used as a node module

```javascript
npm install number-crunch
```

```javascript
var crunch = require("number-crunch");
```

**Example 1**
```javascript
x = [10, 123, 21, 127];
y = [4, 211, 176, 200];
crunch.add(x, y); //[15, 78, 198, 71]
```

The library accepts and returns 8-bit integer arrays which represent artbitrary-precision (big) integers, but internally it uses 28-bit arrays and performs the conversions automatically.

Crunch also converts the between big integer byte-array representation and base-10 strings (a string is used as the Number type could not represent large numbers) using the .stringify() and .parse() functions.


**Example 2**
```javascript
crunch.stringify([1,2,3,4,5,6,7,8,9,0]); // "4759477275222530853120"

crunch.parse("4759477275222530853120");  // [1,2,3,4,5,6,7,8,9,0]
```


Functions
----

Function | Input Parameters | Output
--- | --- | ---
add | x, y | x + y
sub | x, y | x - y
mul | x, y | x * y
div | x, y | x / y
sqr | x | x * x
mod | x, y | x % y
bmr | x, y, [mu] | x % y
exp | x, e, n | x^e % n
gar | x, p, q, d, u, [dp1], [dq1] | x^d % pq
inv | x, y | 1/x % y
cut | x | Remove leading zeroes of x
zero | x | Return zero array of length x
and | x, y | x AND y
or | x, y | x OR y
xor | x, y | x XOR y
not | x | NOT x
leftShift | x, s | x << s
rightShift | x, s | x >>> s
compare | x, y | -1: x < y, 0: x = y, 1: x > y
decrement | x | x - 1
factorial | n | n! [n < 268435456]
nextPrime | x | First prime after x
testPrime | x | Boolean x is prime
stringify | x | String (base 10 representation)
parse | s | Arbitrary-precision integer
transform | x, [toRaw] | Radix conversion

Be carefult with right shift, it is right zero-filled for positive numbers, negative numbers retain their sign.

Miller-Rabin primality testing `mrb`, simple mod `mds` and greatest common divisor `gcd` are also implemented as internal methods not exposed via the Crunch object.

Web Workers
----

Crunch can be loaded in a Web Worker. Instruction messages are sent to the worker in the following format:

```javascript
{func: "", args: []}
```

**Example 3**

```javascript
var crunch = new Worker("crunch.js");
var message = {func: "add", args: [[10, 123, 21, 127], [4, 211, 176, 200]]};

crunch.onmessage = function(m) {
	console.log(m);
};

crunch.postMessage(message);
```