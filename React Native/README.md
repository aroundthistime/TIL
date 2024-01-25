# React Native

### 1. useFocusEffect
- `useFocusEffect` can be used to run side-effects when the screen is focused
- `useFocusEffect` is shares similar logic as `useEffect`, but does not take dependencies array as the second parameter.
- As `useEffect` works, the function passed to `useFocusEffect` could return a callback function the screen is no longer focused
- `useFocusEffect` would be triggered not only when the focused state of the screen changes, but also when the callback function given as parameter changes. Therefore the callback should be memoized with `useCallback` hook.
- To only trigger actions when the screen is "refocused", use reference with false as initial state. (otherwise the side-effect will run for the initial render focus as well)
- In short, the following methods could be used with regards to focus state:
    - `AppState`: Add event listener to `AppState` change to see if the `App` is focused or not.
    - `useFocusEffect`: Set callback actions triggered when focus state of screen changes.
    - `useIsFocused`: Hook which returns boolean value expressing whether the screen is focused or not.