# Useful libraries you can use with Javascript

### 1. Immer
- `Immer` is a library for **creating a next immutable by simply modifying current tree**.
- You can easily create new copy of `Array` or `Object` by mutating the original one (but this does not really mutate the original object. The way you write the code looks like mutating the original value)
- `Immer` provides object called `draft`. By mutating this value as if it contains the original value, `Immer` would create a new object based on the original value and the changes made to `draft`.
- `use-immer` library provides useful hooks of `Immer` logic. (eg. `useImmer`, `useImmerReducer` which you can replace `useState`, `useReducer`)