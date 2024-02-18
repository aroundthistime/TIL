# Context API provided by React

## 1. Context (createContext)
- `Context` can be created by calling `createContext` outside component level.
- **`Context` itself doesn't hold any information, but represents which information (context) it is holding**.
- `Context` can have 3 properties:
    - `Provider`
        - `Provider` enables access to the context (`useContext`) from all the components inside it.
        - `Provider` takes `value` props which is the context value you want to provide to deeper components.
        - `Context` does not provide any feature for updating the `value` internally. If the value needs to be updated during runtime, `Provider` should be wrapped by a component which has the `value` and the update logic as its `state`.

    - `Consumer`
        - `Consumer` is an older way of reading context value.
        - The children of `Consumer` will be a function receiving context value as parameter and render result.
        - The children of `Consumer` will be retriggered and update the result when context value has changed.
        - `Consumer` still does its feature (as of React v18.2.0), but is recommended to use `Provider` + `useContext` which does the same functionality.

    - `displayName` (not important)