# Design patterns that can be used in React

### 1. Adapter Pattern
- Adapter pattern can be applied if there is a mismatch in the data between the one used in frontend and in backend. (not a design pattern specific to React, but could be adapted to any system that requires data conversion)
- `Adapter` receives the raw data and returns the processed version in a way that frontend logic requires.
- `Service` is the place where the actual network communication takes place and obtains the raw data from the server.
- Adapter pattern could be applied in a form of:
    - Class
    - Wrapper function (wrapping service)
    - Wrapper component (which adapts the data internally and passes the processed data as props)