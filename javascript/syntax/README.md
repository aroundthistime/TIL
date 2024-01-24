# Useful Javascript Syntax (or operators, ..)

### 1. Bitwise operators
- Bitwise operators normally give faster result, but could ruin readability of the code unless bitwise operation is clearly required.
- `>>`, `<<`: Used shifting
- `~` (tilde operator): Bitwise NOT
- `~~` (double tilde): Operate tilde twice. Mostly returns the original value. Practical usecases could be:
    - Use instead of Math.floor (`~~` is more performance on speed)
    - Convert undefined or null to 0