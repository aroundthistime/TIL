# Useful tips when using Vite

## 1. Share path alias between bundler and typescript
- `vite-tsconfig-paths` is a plugin which allows vite bundler to share the same path alias given to `typescript compiler` (therefore no redundant effort for setting up path alias is required).
- Call the default imported module of the plugin from `vite.config.ts`.