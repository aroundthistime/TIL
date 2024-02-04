# React Hooks

### 1. useImperativeHandle
- Hook used to customzie the handle exposed as a `ref`.
- `useImperativeHandle(ref, createHandle, dependencies?)`
    - `ref`: reference received from `React.forwardRef`.
    - `createHandle`: function which takes no arguments and returns *ref handle* to expose. Usually returns an object with methods to expose.
    - `depdencies`: Optional dependencies for putting *reactive values (props, state, variables or functions declared directly inside the component)* that the `createHandle` uses inside. If one of the values inside this array is updated, `createHandle` will be re-executed and return new handle object.

- Possible use cases:
(1) Expose method unrelated to a certain element<br>
(2) Expose limited method of a certain element (eg. create `ref` inside child component which references the *actual element* and only expose `click` method of the element to the parent component)<br>