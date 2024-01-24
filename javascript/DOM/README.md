# DOM (Document Object Model)

### 1. How to validate whether element is focused or not
- Store whether the element is focused or not inside React state and update inside event handlers.
- Compare the element with `document.activeElement`

### 2. Canvas image processing with cross origin resources
- Processing an image data from cross-origin is prevent due to security issues (The canvas has been tainted by cross-origin data)
- The issue can be resolved by the same approach as other CORS issues. (`crossOrigin` attribute could also be used for images)