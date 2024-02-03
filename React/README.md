# React

### 1. Execution order
- useEffect statements can only be called after the component is mounted (whereas the logic inside top scope of component will be called immediately)
- Child component is rendered after Parent component (Component Waterfall)
- useEffect or useLayoutEffect statements are executed in a reverse order (child => parent)


### 2. JSX (Javascript XML)
- JSX is an XML-like extension to ECMAScript which enables writing HTML inside Javascript.
- JSX is basically syntax sugar of `React.createElement` (which you can use to create element inside React without using JSX)
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