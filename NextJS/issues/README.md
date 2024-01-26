# Next.js issues and how to resolve them

### 1. eslint configuration file not being created
- Issue: Next.js is not creating eslint configuration file after running `next lint`
- How to resolve:
    - Temporarily create empty `pages` directory in the root of the project.
    - Run `next lint` (configuration file would be created fine)
    - Delete temp `pages` directory
- [Reference link](https://github.com/vercel/next.js/issues/50761#issuecomment-1666057683)