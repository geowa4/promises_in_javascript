footer: Wednesday, 5 August 2015
slidenumbers: true

# [fit] Promises in JavaScript

### George Adams, IV

^ Promises in JavaScript are Monads.

---

# What are Monads?

^ The obvious follow up question.

^ Let's dig just a little deeper.

---

# Monad = Design Pattern

^ Everyone is probably familiar with the Gang of Four and patterns like Factory, Facade, and Observer.

^ Those are all patterns discovered in OO workplaces.

^ Monad is just another pattern that came out of the mathematically focused FP ecosystem.

---

# Monad Laws

^ There are three Monad laws, but I'll forego the pure definition to make it more consumable.

---

# A Function

There must be a function to take a simple value and return a promise.

```javascript
var myPromise = Q(7)
```

^ Using the Q promise library, this is how you can take a 7 and turn it into a Promise of 7.

^ We'll see why that's useful later. I promise.

---

# Binding

There must be a way to work with the value of the Promise.

```javascript
function doubleIt (value) {
    return Q(value * 2)
}

myPromise
    .then(doubleIt)
```

^ Notice that the function must return a Promise.

^ `doubleIt` takes an `Int` and returns `Promise<Int>`.

---

# Chainable

Promises must be chainable.

```javascript
myPromise
    .then(doubleIt)
    .then(function (value) {
        console.log(value)
    })
```

---

# JavaScript is Not Typed

Promises must be a little forgiving.

```javascript
function doubleIt (value) {
    return Q(value * 2)
}

function doubleItPrime (value) {
    return value * 2
}
```

---

# Left Identity

```javascript
Q(7).then(doubleIt) == doubleIt(7)
```

^ That statement won't return `true` in the console, but they're functionally equivalent.

---

# Right Identity

```javascript
myPromise.then(Q) == myPromise
```

---

# Associativity

```javascript
myPromise.then(foo).then(bar) ==
myPromise.then(function (value) { foo(value).then(bar) })
```

^ Those are the Monad laws

---

# Usage

---

# Why is this Helpful?

 - Deferment
 - Immutable data
 - Repeatability

---

# Deferment

`foo` executes asynchronously when `http()` returns.

```javascript
http()
    .then(foo)

bar()
```

^ `bar` executes before `foo`.

---

# Immutable Data

`myPromise` can't be changed once it's data has been set.

---

# Repeatability

```javascript
myPromise
    .then(foo)

myPromise
    .then(bar)
```

---

# The Real World

...is full of failure.

---

# Two Paths

```javascript
function happy (value) {
    return Q(value * 2)
}

function sad (reason) {
    // default to 7
    return Q(7)
}

http()
    .then(happy, sad)
    .then(function (value) {
        console.log(value)
    })
```

^ In languages like Haskell, you'd pass in one function and use pattern matching.

^ I like to think of it as passing in two parts of the function body where Q does the pattern matching for me.

^ To go on the happy path, just return a value.

^ To go on the sad path, throw something.

---

# Path Traversal

```javascript
http()
    .then(function (value) { throw "I can't handle a " + value + "!" })
    .then(
        function (value) { /* does not execute */ }),
        function (reason) {
            console.log(reason)
            return 10
        }
    )
    .then(
        function (value) { return value * 2 },
        function (reason) { /* does not execute */ }
    )
    .then(function (value) { console.log(value) })
```

---

# Further Reading

 - [Either](https://hackage.haskell.org/package/category-extras-0.52.0/docs/Control-Monad-Either.html)
 - [Q](https://github.com/kriskowal/q/wiki/API-Reference)
 - [jQuery Deferred](http://api.jquery.com/category/deferred-object/)
