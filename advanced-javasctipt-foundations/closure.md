## Closure

Observational definition:  
**Closure is the ability of a function to "remember" its lexical scope even when it is executed outside that lexical scope.**

### Examples

```js
function outer() {
  var counter = 0;

  function inner() {
    console.log(counter);
    counter++;
  }

  return inner;
}

var myFunc1 = outer();
myFunc1(); // 0
myFunc1(); // 1
myFunc1(); // 2

var myFunc2 = outer();
myFunc2(); // 0
myFunc2(); // 1
```

```js
function foo() {
  var bar = 'bar';

  return function() {
    console.log(bar);
  };
}

function bam() {
  foo()(); // "bar"
}

bam();
```

```js
function foo() {
  var bar = 'bar';

  setTimeout(function() {
    console.log(bar);
  }, 1000);
}

foo();
```

```js
function foo() {
  var bar = 'bar';

  $('#btn').click(function(evt) {
    console.log(bar);
  });
}

foo();
```

```js
function foo() {
  var bar = 0;

  setTimeout(function() {
    console.log(bar++);
  }, 100);
  setTimeout(function() {
    console.log(bar++);
  }, 200);
}

foo(); // 0 1
```

```js
function foo() {
  var bar = 0;

  setTimeout(function() {
    var baz = 1;
    console.log(bar++);

    setTimeout(function() {
      console.log(bar + baz);
    }, 200);
  }, 100);
}

foo(); // 0 2
```

```js
for (var i = 1; i <= 5; i++) {
  setTimeout(function() {
    console.log('i : ' + i);
  }, i * 1000);
}
```

When you run the above code it will print `i : 6` five times.  
There are two reasons why:

* `i` is `6` at the end of the loop and all these timers run a lot later, so it's going to print `6` (surface reason)
* Why don't we get a new `i` for each iteration? Because we use the `var`. We made one `i` attached to the existing scope (deeper reason)

To create a new `i` every time we can either replace `var` with `let`, or use an `IIFE` like so:

```js
for (var i = 1; i <= 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log('i : ' + i);
    }, i * 1000);
  })(i);
}
```
