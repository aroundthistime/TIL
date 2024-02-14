# Useful tips I can use with React

## 1. Selector without rerendering
- Subscribing property inside global state could be achieved without triggering rereders by utilizing `ref`.
```
import { useRef } from 'react';
import type { MutableRefObject } from 'react';
import { useSelector } from 'hooks';
import type { RootState } from 'app/store';

// useSelector without re-render
export default function useSelectorRef<T = unknown>(
  selectHandler: (state: RootState) => T
): MutableRefObject<T> {
  const ref = useRef<T>();

  // Utilize compare function to update the reference
  useSelector<T>(selectHandler, (_, b) => {
    ref.current = b;
    return true;
  });

  return ref as MutableRefObject<T>;
}
```

## 2. React.forwardRef with Generic Components
- `Generic types` of `Generic components` could be lost (perceived as `unknown`).
  - `Generic components` are often used to implement a generic table for supporting various items.
- This could be resolved by wrapping the component with fixed type `HOC`.

```
function fixedForwardRef<T, P = {}>(
  render: (props: P, ref: React.Ref<T>) => React.ReactNode
): (props: P & React.RefAttributes<T>) => React.ReactNode {
  return React.forwardRef(render) as any;
}

export default fixedForwardRef(BaseComponent);
```