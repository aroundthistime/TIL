# Recoil

## 1. RecoilRoot
- Wrapper component at the root.
- Based on `Context API` internally.

## 2. Atom
- `Atom` represents a piece of `state`.
- `Atom` is usually defined in a dedicated module and components (or hooks) will import and use it. (the same applies for other recoil states like `selectors`, `families`, ..)
- `Atom` has one required option:
    - `key`: key string to distinguish the `atom` from others.
- Hooks to utilize with `atoms`:
    - `useRecoilState`: similar to `useState` (returns of array of `state` and `setState`, so a combination of `useRecoilValue` + `useSetRecoilState`)
    - `useRecoilValue`: Hook which returns `state` of `useState`.
    - `useSetRecoilState`: Hook which returns `setState` of `useState`. Just like `setState`, this doesn't trigger re-render due to state updates.


## 3. Selector
- `Selector` represents a piece of `derived state` from `Atom`.
- `Selector` takes 2 required options:
    - `key`: key string to distinguish the `selector` from others.
    - `get`: A pure function to derive the state from another `atom` or `selector`. `Selector` will internally track the states that this `selector` is deriving from and will re-run this method on their changes.
- The usage of `Selector` is very much similar to `Atom`, but it is important that some hooks are compatible with `writable states` (eg. `useRecoilState`).
    - `Atom`: `Writable` and `Readable`
    - `Selector`: `Readable` by default, but is `writable` only when `set` option is given.