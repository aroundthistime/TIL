# React Hooks

### 1. useState
- Hook used to add *state* inside a component.
- The return value of useState is an array containing `state value` and `setState`.
- The parameter of `setState` can be one of two types:
    - `nextState`: the value to set as state
    - `updater function`: function which takes previous pending state as argument and returns the `nextState`. `updater function` should be a pure function (in *Strict Mode*, `updater function` is executed twice to validate this)
- `setState` only update the value **for the next render**, so the value inside state would still be the pending value after calling `setState`.
- `setState` would not trigger rerendering if `nextState` is considered to be equal to the previous state based on `Object.is` comparison.
- `React` **batches state updates** and therefore the updates are made after all event listeners have run (`React 18`) and calling `setState` functions are complete. This behavior could be ignored by updating the screen earlier using `flushSync` in rare cases.
- If the logic of `state` becomes complex and **setting next state is calculated from previous state of another state, combining them into one and using reducer could be useful**.
- `setState` using `updater function` is batched and therefore can calculate `nextState` based on the value updated inside the batch.
```
const [count, setCount] = useState(0);

const doubleIncreaseHandler = () => {
    // count becomes 1
    setCount(count+1);
    setCount(count+1);

    // count becomes 2
    setCount(actualCount => actualCount + 1);
    setCount(actualCount => actualCount + 1); // actual count here is 1
  };
```


### 1. useImperativeHandle
- Hook used to customzie the handle exposed as a `ref`.
- `useImperativeHandle(ref, createHandle, dependencies?)`
    - `ref`: reference received from `React.forwardRef`.
    - `createHandle`: function which takes no arguments and returns *ref handle* to expose. Usually returns an object with methods to expose.
    - `depdencies`: Optional dependencies for putting *reactive values (props, state, variables or functions declared directly inside the component)* that the `createHandle` uses inside. If one of the values inside this array is updated, `createHandle` will be re-executed and return new handle object.

- Possible use cases:
(1) Expose method unrelated to a certain element<br>
(2) Expose limited method of a certain element (eg. create `ref` inside child component which references the *actual element* and only expose `click` method of the element to the parent component)<br>