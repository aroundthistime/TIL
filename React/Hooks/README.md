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


### 3. useEffect
- Hook for synchronizing component to an external system (eg. add window event listener, DB connections, ..)
- `setup`: The first parameter of `useEffect`, which is an effect logic function executed when one of the `dependencies array` changes.
    - `setup` could return a `clean up` function using old values.
- `useEffect` with empty dependency array will run on every render.
    - For most of the case, this could be replaced with direct call outside `useEffect`.
    - **For operations that have to run on every render and are synchronous, executing the logic on the component level would affect performance and rendering** (because the operation would be done within rendering process). This could be resolved with empty `useEffect` since `useEffect` statements are executed after render is complete.
- Excution process of `useEffect`:<br>
(1) Component is rendered<br>
(2) `setup` inside `useEffect` will be called.<br>
(3) Component is rerendered<br>
(4) If one of the dependencies has changed, `cleanup function` will run based based on old values.<br>
(5) `setup` logic is executed with new values.<br>
(6) When component gets removed from DOM, React runs `cleanup function`<br>

- `useEffect` only runs on the client (not on the server)
- For all `reactive variables` inside `Effect`, it is recommended to put them inside dependencies array of `useEffect`. But for the `states` being updated often based on the previous `state`, this could drop performance. In that case the `state` could be removed from dependencies array by using `updater function` inside `setState`.
- Cases that you shouldn't use `useEffect`:
    - **Computed state** (value decided by another state's value)
        - If each are stored as a `state` and the computed one is updated by `useEffect`, the process would go like (state changes → component rerenders → useEffect runs and updates computed state → component rerenders). So component has to be rerendered 2 times for a single update in the logic.
        - If the computation logic is not complex, doing the computation in the component level is enough. (compute on every render)
        - If the computation logic is complex, memoize the state using `useMemo` (unlike `useEffect`, **this only triggers single rerendering**)
    
    - **Resetting states on props change**
        - Even if a component is rerendered because props has changed, it does not mean that component is unmounted and therefore all the states all being reset.
        - **For resetting the `state` of the component based on the `props` update, set `key` of the component to the `props` which triggers the reset**.

    - **Updating states on props change**
        - If `state` has to be updated based on `props` and it is implemented using `useEffect`, there will be 2 renderings for single update (*one due to props update*, *one due to state update*)
        - Just like `state` computed from other `states`, `state` computed from `props` is better computated in component level with single rerendering. (or use `useMemo` if the computation is heavy)

    - **Event Handlers**
        - Event handling logic should be done inside event handler with full context of the event, not inside Effects.



### 1. useImperativeHandle
- Hook used to customzie the handle exposed as a `ref`.
- `useImperativeHandle(ref, createHandle, dependencies?)`
    - `ref`: reference received from `React.forwardRef`.
    - `createHandle`: function which takes no arguments and returns *ref handle* to expose. Usually returns an object with methods to expose.
    - `depdencies`: Optional dependencies for putting *reactive values (props, state, variables or functions declared directly inside the component)* that the `createHandle` uses inside. If one of the values inside this array is updated, `createHandle` will be re-executed and return new handle object.

- Possible use cases:
(1) Expose method unrelated to a certain element<br>
(2) Expose limited method of a certain element (eg. create `ref` inside child component which references the *actual element* and only expose `click` method of the element to the parent component)<br>