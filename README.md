# Function.prototype.compose TC39 Proposal

| | |
|-|-|
| **Status** | Stage 0 Strawman |
| **Champion** | None |
| **Author** | Simon Staton |
| **Last Updated** | 29th August 2017 |

## Summary
In functional programming function composition is an act or mechanism to combine simple functions to build more complicated ones. Like the usual composition of functions in mathematics, the result of each function is passed as the argument of the next, and the result of the last one is the result of the whole.

```
function composed(arg) {
  return c(b(a(arg)))
}
```

## Motivation
Function composition is a pattern that is commonly adopted in JavaScript, it can be utilised to reduce inline nesting and code complexity and is powerful when implementing functional architectures.

Currently there is no native implementation to support this and as such a range of libraries provide this as a utility with differentiating approaches/designs. The actual implementation of this method is trivial but the overall objectives are the same.

## Requirements

This prioposal should provide a readable and maintainable approach to composing functions without a depedency on wrapping the original function call. It should provide some way of handling the control flow and also mapping arguments at a function level. The initial submission of this proposal is just for .compose() but further discussions should be had surrounding .composeAfter() .composeBefore() .composeAround()

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

Although all of these use cases are solved via nesting function calls this does not provide a clean or maintainable way of composing the functions. It could be considered counter intuitive to...

## Existing Implementations
* [lodash](https://lodash.com/docs/4.17.4#flow)
* [Ramda](http://ramdajs.com/docs/#compose)
* [Java](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html#compose-java.util.function.Function-)
* [Haskell](https://wiki.haskell.org/Function_composition)
* [Python](https://docs.python.org/3.1/howto/functional.html#the-functional-module)

## Mock Implementation

The following is an example implementation, functions are chained right to left in the order they are invoked `compose(c, b, a)`, `c(b(a()))`

```javascript
Function.prototype.compose = function(...fns) {
    fns.unshift(this);
    return fns.reduce((chained, fn) => (...args) => chained(fn(...args)))
};
```

#### Usage

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

Current alternatives

Other alternatives

Infix Operators

## Challenges

Performance issues?

Implementation issues

Possible user errors

## Discussions

Link off to other proposals and discussions surrounding this

## Changelog
