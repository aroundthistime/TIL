# Useful Javascript Syntax (or operators, ..)

### 1. Bitwise operators
- Bitwise operators normally give faster result, but could ruin readability of the code unless bitwise operation is clearly required.
- `>>`, `<<`: Used shifting
- `~` (tilde operator): Bitwise NOT
- `~~` (double tilde): Operate tilde twice. Mostly returns the original value. Practical usecases could be:
    - Use instead of Math.floor (`~~` is more performance on speed)
    - Convert undefined or null to 0

### 2. Generator function
- `Generators` can return(*yield*) multiple values (one after another on demand) unlike regular functions which are executed once and returns only one value.
- Generator functions (`GeneratorFunction object`) can be defined with `function*` declaration which is used to create `Generators`.
```
function* generator(i) {
    yield i;
    yield i + 10;
}

const gen = generator(10); // note that this does not require new operator!
```
- `Generator functions` calls will return a new `Generator` object.
    - `Generator` is not executed immediately but only executed on demand when its `next` method has been called.
    - When `next` method is called, the code runs till the next `yield` statement and yields the result of the `yield` statement.
    - Every time `next` method is called, it will run after the previous `yield` till the next `yield` or `return` statement.
    - The result object of `next` has following properties:
        - `value`: Result of the execution (value of `yield` or `return` statement)
        - `done`: Whether the generator has yielded every values
    - The breakpoints and obtainable values inside `Generator` can be set by either `yield` or `return` statement. Difference is that `return` would terminate the operation of the `Generator`, and the logic afterwards will not be executed. And just like regular functions, missing `return` is same as `return undefined` at the end. Therefore, when a `generator` only includes `yield` statements, `done` property of the `next()` result will only turn true after the last `yield`, with `value: undefined`. Therefore to if the value of the final `Generator` operation is important, it is recommended to use `return` statement at the end instead of using `yield`.
    - `Generator` can include `try-catch` statement inside. (uncaught error inside `Generator` will finish the `Generator`)

- `Generator function` can also be a `method` of a `class`
```
class A {
    *generator() { // The name doesn't have to be generator
        yield 1;
        yield 2;
    }
}
```

- Difference between `yield` and `yield*`:
    - `yield*` is used to delegate the iteration to another `generator` or `iterable`
    - `yield* iterable` === `for (const value of iterable) yield value;`

- `Generator Function` was introduced in *ES6*.
- `async/await` uses the behavior of `Generator Function` internally:
    - Code inside `async functions` will be converted to `Promises`, which are chained using `next` method of `Generator` .
    - The tranpiling of `Babel` will change `async` function into function which returns `Promise` in the way mentioned above.
```
// Original Code
const b = async() => {
    console.log('b1');
    await a();
    console.log('b2');
}

// Transpiled Code
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
  try {
    var info = gen[key](arg);
    var value = info.value;
  } catch (error) {
    reject(error);
    return;
  }
  if (info.done) {
    resolve(value);
  } else {
    Promise.resolve(value).then(_next, _throw);
  }
}

function _asyncToGenerator(fn) {
  return function () {
    var self = this,
      args = arguments;
    return new Promise(function (resolve, reject) {
      var gen = fn.apply(self, args);
      function _next(value) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, 'next', value);
      }
      function _throw(err) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, 'throw', err);
      }
      _next(undefined);
    });
  };
}

function a() {
  // some task
}

function b() {
  return _b.apply(this, arguments);
}

function _b() {
  _b = _asyncToGenerator(function* () {
    console.log('b1');
    yield a();
    console.log('b2');
  });
  return _b.apply(this, arguments);
}

b();
```