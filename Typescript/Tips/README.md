# Tips you could use for typescript

## 1. Typing env variable
```
declare global {
    namespace NodeJS {
        interface ProcessEnv {
            API_KEY: string;
        }
    }
}

process.env.API_KEY; // string
```