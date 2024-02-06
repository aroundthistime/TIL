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
- If the logic of `state` becomes complex and **setting next state is calculated from previous state of another state, combining them into one and using reducer could be useful** (improves readability of the code).
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


### 2. useReducer
- Hook for adding a `reducer` to a component.
- `useReducer` takes 3 arguments:
    - `reducer`: function which takes `state` and `action` to return the `newState`.
        - `reducer` should be a pure function which calculates the `newState` only using `previous state` and `action` (**should not include any side effects like fetch or asynchronous logic**). This is because **`reducers` do not run immediately. They run during rendering and the actions are queued until next render**.
        - `reducer` **should return a new value instead of mutating the existing `state`** (`state` should be `read-only`).
    - `initialArg`: value used to calculate the **initial state** (so practically **this is not the initial state**)
    - `init`: (*optional*) initializer function which takes `initialArg` and returns **initial state**. (**if this is unprovided, `initialArg` would just become the `initial state`**)
- `useReducer` returns an array of two values:
    - `current state`
    - `dispatch`

- `dispatch`: function which can update state (which would trigger rerender)
    - `dispatch` takes `action` as argument which expresses the information on how you want to update the state. (**`action` can be any type of data but by convention, `action` is an `object` with `type` property + extra properties for further information**)
    - The behavior of `dispatch` is pretty much similar to `setState`:
        - `state` variable is updated on the next render. Therefore referencing the `state` right after `dispatch` call would still provide the old value.
        - If the updated `state` is same as the old `state` (`Object.is`), `React` would prevent unnecessary rerendering.
        - `state` update will be batched (and you can also use `flushSync` to force update eariler)

- Pro tips:
    - **Use `init` function if `initial state` needs to be calculated (instead of calling calculator inside `initArg`)**
        - If the `initArg` is passed as function call (`eg. calculateInit(arg)`), the execution of the function would be called on every rerender, although the value would only be actually used on the initial render.
        - If the `init` function is used, the execution of the initializer function would be called only once.
        - Defining the `init` function would still have to done outside the component.
    - **Throw error for inappropriate `actions`**
        - The return value of `reducer` function will be the `next state`. But if inappropriate `action` is given and the `reducer` does not find any corresponding logic, the state could be `undefined` by default return value. Since `action` is normally an object with type and `reducer` uses `switch` statement, add a `default` statement which would throw error. This would prevent incorrect `action` from messing up the `state`.
    - **Using immer inside reducer to update state easily**
        - `Immer` is a library which enables updating an object while respecting immutability.
        - `Immer` provides `draft` object. Based on this, `Immer` internally creates a copy of state based on the changes given to `draft`.
        - `useImmerReducer`: hook provided by `use-immer` library. This provides `draft` instead of `state` as the first argument of `reducer` function. You can update `draft` object directly and the updated object doesn't have to returned either. For example the following tasks are all possible (push new element to array, change property of array, change item inside array by index)
        - Unless `return` state is explicitely given, `reducer` of `useImmerReducer` will set result of `draft` to the `next state`. But if the desired `next state` cannot be obtained by just mutating `draft`, you could return the value (eg. filtering an array)
        - `Redux Toolkit` internally uses `Immer` library. This is how you can mutate the state directly and returning `next state` if not required.



### 3. useState vs useReducer
- **Code size + Readability**: this depends on the case. Normally `useState` requires less code and is easy to understand if the logic is not complex. But if the logic about the state gets complex, `useReducer` could take the advantage (since the actual state update logic could be consolidated).
- **Debugging**: In case of `useState`, it's difficult to catch where state was updated incorrectly. In case of `useReducer`, it can be easily traced inside `reducer` function.
- **Testing**: Since `reducer` is a pure function, it can be isolated and tested on its own.
- There isn't an absolute rule on which one is better to another. For performance and the behavior, `useState` and `useReducer` are pretty much similar and it's OK to choose one based on personal (or team's) preference.


### 4. useEffect
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


### 5. useLayoutEffect
- Similar to `useEffect`, but the **`Effect` is fired before browser repaint**.
- `useLayoutEffect` runs before `repaint` but after `layout` or `reflow`. Therefore you can utilize `useLayoutEffect` to update measure layout before screen repaint.
```
// Example code using useLayoutEffect to set tooltip height based on layout measurement
const Tooltip = () => {
    const ref = useRef(null);
    const [tooltipHeight, setTooltipHeight] = useState(0); // You don't know real height yet (fill dummy value)

    useLayoutEffect(() => {
        const { height } = ref.current.getBoundingClientRect();
        setTooltipHeight(height); // Re-render now that you know the real height
    }, []);
}
```

- The detailed steps on how the above code works:
(1) `Tooltip` renders with dummy initial value.<br>
(2) `React` puts `Tooltip` inside `DOM` and runs `Effect` inside `useLayoutEffect`.<br>
(3) `useLayoutEffect` measures actual height of tooltip content and triggers immediate re-rendering.<br>
(4) `Tooltip` is rerendered with actual height.<br>
(5) `React` updates it to the DOM and **browser finally displays the `Tooltip`**<br><br>

- `useLayoutEffect` could be used for the following purposes:
    - **Render initial content** (because **`useLayoutEffect` could trigger immediate re--rendering before `(re)paint`, the user cannot see the initial render result**)
    - **Measure the layout before browser paints** the screen (and possibily use it to render)

- `useLayoutEffect` runs synchronously and blocks `repaint` which could drop performance. Therefore in most of the cases it is recommended to use `useEffect`.
- `useLayoutEffect` cannot run on the server since there is not layout information.
    - `useIsomorphicLayoutEffect`: Hook provided by `usehooks-ts` library to use `useLayoutEffect` in server side. In the server, it is `useEffect` and on the client it becomes `useLayoutEffect`.




### 1. useImperativeHandle
- Hook used to customzie the handle exposed as a `ref`.
- `useImperativeHandle(ref, createHandle, dependencies?)`
    - `ref`: reference received from `React.forwardRef`.
    - `createHandle`: function which takes no arguments and returns *ref handle* to expose. Usually returns an object with methods to expose.
    - `depdencies`: Optional dependencies for putting *reactive values (props, state, variables or functions declared directly inside the component)* that the `createHandle` uses inside. If one of the values inside this array is updated, `createHandle` will be re-executed and return new handle object.

- Possible use cases:
(1) Expose method unrelated to a certain element<br>
(2) Expose limited method of a certain element (eg. create `ref` inside child component which references the *actual element* and only expose `click` method of the element to the parent component)<br>