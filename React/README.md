# React

### 1. Execution order
- useEffect statements can only be called after the component is mounted (whereas the logic inside top scope of component will be called immediately)
- Child component is rendered after Parent component (Component Waterfall)
- useEffect or useLayoutEffect statements are executed in a reverse order (child => parent)


### 2. JSX (Javascript XML)
- JSX is an XML-like extension to ECMAScript which enables writing HTML inside Javascript.
- JSX is basically syntax sugar of `React.createElement` (which you can use to create element inside React without using JSX by creating a virtual DOM element)
- `JSX` code is converted to corresponding `React.createElement` code by `Babel`.
- `JSX` approach
```
<h1 className='title'>Hello</h1>
```
- `React.createElement` approach
```
React.createElement(
    'h1',
    { className: 'title' },
    'Hello'
)
```
- `React.createElement` takes 3 parameters:
    - element type
    - props
    - children
- Process of rendering JSX element (*before React 17*):
(1) JSX code is written.<br>
(2) JSX code is converted to `React.createElement` by `Babel` (Reason why you needed to import `React` inside files with `jsx`, because `createElement` is required for compiling)<br>
(3) `React.createElement` code will create *Virtual DOM element* with its parameters.<br>
(4) `ReactDOM` library will render *Virtual DOM elements* inside actual DOM container. (`ReactDOM.render`)<br><br>

- Process of rendering JSX element (*starting from React 17*)
(1) JSX code is written.<br>
(2) During compile process, `Babel` will import `react/jsx-runtime` automatically inside files containing `jsx` and call method from the imported library which converts `jsx` into *React Virtual Element*. (<u>`jsx` compiling no longer uses `React.createElement`</u> which is why importing `React` is no longer mandatory)<br>
(3) `ReactDOM` library will render *Virtual DOM elements* inside actual DOM container.

- React 17 still provides `React.createElement` in case you want to create elements without using `jsx`.


### 3. Pure Component
- Components which always render the same output for the same `state` and `props`.
- For function components:
    - `props`, `React.memo` could be used to memoize the component and prevent from rerendering when same `props` is given. (`React.memo` is a HOC)
    - `state`, function component can compare the state and optimize rerendering on its own.
- For class components:
    - components extending `React.PureComponent` instead of `React.Component` becomes pure components and will do shallow comparison of `props` and `state` to prevent unnecessary rerendering (based on `shouldComponentUpdate` lifecycle method)
- During development, React provides *Strict Mode* where functions of each components are executed twice. This is to validate whether a component is pure or not.

