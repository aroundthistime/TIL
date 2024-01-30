# Debugging in browser

### 1. Watch DOM snapshot
(1) Open `Elements` tab in devtools<br>
(2) Right click certain element and click `Break on`<br>
(3) Select breakpoints based on DOM snapshot (eg. element removed, subtree changed)


### 2. Pause URL change in SPA
```
const dbg = () => {
  debugger;
};
history.pushState = dbg;
history.replaceState = dbg;
window.onhashchange = dbg;
window.onpopstate = dbg;
```

- Throw debugger when some routing event happens.
- The above method would not work if page has been unmounted with native calls (eg. `window.location.replace`)