# React

### 1. Execution order
- useEffect statements can only be called after the component is mounted (whereas the logic inside top scope of component will be called immediately)
- Child component is rendered after Parent component (Component Waterfall)
- useEffect or useLayoutEffect statements are executed in a reverse order (child => parent)