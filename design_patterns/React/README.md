# Design patterns that can be used in React

### 1. Adapter Pattern
- Adapter pattern can be applied if there is a mismatch in the data between the one used in frontend and in backend. (not a design pattern specific to React, but could be adapted to any system that requires data conversion)
- `Adapter` receives the raw data and returns the processed version in a way that frontend logic requires.
- `Service` is the place where the actual network communication takes place and obtains the raw data from the server.
- Adapter pattern could be applied in a form of:
    - Class
    - Wrapper function (wrapping service)
    - Wrapper component (which adapts the data internally and passes the processed data as props)

- Method of implementing `adapter` with `React Query`:
    - Starting form `v3`, query hooks of `React Query` provides `select` option to **select or transform the data**.
    - The result of the query hook be able to `data` in a transformed way.
    - `select` function only runs when proper data exists, therefore validating whether the fetch function has succeded or has been triggered is not required.
    - The query with `select` function will share the same cache with the one without `select` as long as query key is the same. Therefore there is no need to worry about the performance of sending redundant requests.
    - If `select` function is defined `in-line`, this will run on every rerender. Therefore if the calculation cost is expensive, it is recommended to **define the function outside React** or **memoize the function with `useCallback`**.
    - Setting `notifyOnChangeProps` to `['data', 'error']` could be used to optimize rerendering to be triggered only when the selected data has changed.