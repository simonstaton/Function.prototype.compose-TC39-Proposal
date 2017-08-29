# Function.prototype.compose TC39 Proposal

| | |
|-|-|
| **Status** | Stage 0 Strawman |
| **Champion** | None |
| **Author** | Simon Staton |
| **Last Updated** | 29th August 2017 |

## Summary
The act of combining simple functions to build more complicated ones. Like the usual composition of functions in mathematics, the result of each function is passed as the argument of the next and the result of the last one is the result of the whole.

```
abc = c(b(a()))
```

## Motivation
Function composition is a pattern that is commonly adopted in JavaScript, and when used correctly it can reduce inline nesting and code complexity. This can be a powerful design pattern when implementing functional architectures.

Currently there is no native implementation to support this and as such a range of libraries provide this as a utility with differentiating approaches/designs. The actual implementation of this is trivial but the overall objectives are the same.

## Requirements

This proposal should provide a readable and maintainable approach to function composition, it should outline the reasonings for this design pattern and any possible downfalls. The goal of this feature should be to provide a lightweight method to compose functions on the fly.

The implementation itself should provide a clear representation of the control flow and will compose functions from right to left as to follow the current call stack c(b(a())

## Use Cases

**Functional Building Blocks**
```javascript
const car = car => startMotor(useFuel(turnKey(car)));

const electricCar = car => startMotor(usePower(turnKey(car)));
```

**Control Flow Management**
```javascript
// request -> filter -> sort -> truncate
const getData = query => truncate(sort(filter(request(query))));
```

**Argument Assignment**
```javascript
const sortBy = 'date';
const getData = query => truncate(sort(filter(request(query)), sortBy));
```

Although all of these use cases are solved via nesting function calls this does not provide a clean or maintainable way of handling function composition. Why? Expand on this point

## Example Implementations
* [JavaScript lodash](https://lodash.com/docs/4.17.4#flow)
* [JavaScript Ramda](http://ramdajs.com/docs/#compose)
* [Ruby](http://www.rubydoc.info/github/rubyworks/facets/Proc:compose)
* [Java](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html#compose-java.util.function.Function-)
* [Perl 6](https://docs.perl6.org/routine/compose)
* [Haskell](https://wiki.haskell.org/Function_composition)
* [Python](https://docs.python.org/3.1/howto/functional.html#the-functional-module)
* [Lisp](http://www.cliki.net/COMPOSE)

## ES6 Mock Implementation

```javascript
Function.prototype.compose = function(...fns) {
    fns.unshift(this);
    return fns.reduce((chained, fn) => (...args) => chained(fn(...args)))
};
```

## Usage

```javascript
// Functional Building Blocks
const car = startMotor.compose(useFuel, turnKey);
const electricCar = startMotor.compose(usePower, turnKey);

// Control Flow Management
const getData = truncate.compose(sort, filter, request);

// Argument Assignment
const sortBy = 'date';
const getData = truncate.compose(sort, filter.bind(data, sortBy), request);
```

## Alternatives

#### Higher Order *

#### Generators

#### Operators

Infix Operator - expand on pipes

## Challenges

#### Performance

#### Implementation

## Discussions
https://esdiscuss.org/topic/native-function-composition
https://esdiscuss.org/topic/function-composition-syntax
https://esdiscuss.org/topic/pipe-operator-for-javascript

## Changelog

