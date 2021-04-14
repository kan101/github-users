# vue3

Vue 3 with Vue Router 4, Typescript 4, Webpack 5, Prettier.

## Architecture

```text
├─ public           // static assets.
├─ service          // commands and webpack configurations.
├─ src
│  ├─ assets        // assets such as images or font files.
│  ├─ components    // universal Vue components.
│  ├─ router        // view's routers config.
│  ├─ typings       // typescript .d.ts files.
│  └─ views         // pages.
```

## Commands

```bash
# Start development server.
yarn serve

# Compile production bundle.
yarn build
```
