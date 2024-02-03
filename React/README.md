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
- Process of rendering JSX element:
(1) JSX code is written.<br>
(2) JSX code is converted to `React.createElement` by `Babel`<br>
(3) `React.createElement` code will create *Virtual DOM element* with its parameters.<br>
(4) `ReactDOM` library will render *Virtual DOM elements* inside actual DOM container.