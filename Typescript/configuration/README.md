# Typescript compiler configuration

## 1. jsx flag
- Specifies what JSX code is generated.
- Mostly used values:
    - `preserve`: preserve the `jsx` code and do not add any information.
    - `react-jsx`: use modern `jsx` transform (`React 17` or higher)
    - `react`: use legacy `jsx` transform (based on `React.createElement`)