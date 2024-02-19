# Recoil

## 1. RecoilRoot
- Wrapper component at the root.
- Based on `Context API` internally.

## 2. Atom
- `Atom` represents a piece of `state`.
- `Atom` is usually defined in a dedicated module and components (or hooks) will import and use it. (the same applies for other recoil states like `selectors`, `families`, ..)
- Hooks to utilize with `atoms`:
    - `useRecoilState`: similar to `useState` (returns of array of `state` and `setState`, so a combination of `useRecoilValue` + `useSetRecoilState`)
    - `useRecoilValue`: Hook which returns `state` of `useState`.
    - `useSetRecoilState`: Hook which returns `setState` of `useState`. Just like `setState`, this doesn't trigger re-render due to state updates.