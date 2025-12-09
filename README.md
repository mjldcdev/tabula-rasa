## tabula‑rasa

A **Next.js** starter template with TypeScript, ESLint, Prettier, Tailwind CSS, and full workspace configuration.

---

### Requirements

| Item        | Minimum                           |
| ----------- | --------------------------------- |
| **Node.js** | 20.9                              |
| **OS**      | macOS, Windows (incl. WSL), Linux |

> **Reference:** <https://nextjs.org/docs/app/getting-started/installation#system-requirements>

---

### Tech Stack

- **Next.js** (app‑router)
- **TypeScript**
- **ESLint** – plugins: `["check-file"]`
- **Prettier** – plugins: `["@trivago/prettier-plugin-sort-imports", "prettier-plugin-tailwindcss"]`
- **Tailwind CSS**

---

### Setup Guide

#### 1️⃣ Scaffold the base project

```bash
# create the app
pnpm create next-app@latest next-base

# move into the directory
cd next-base

# verify the dev server starts
pnpm dev
```

Commit the initial files once the server runs successfully.

**Reference:** <https://nextjs.org/docs/app/getting-started/installation#quick-start>

#### 2️⃣ Workspace settings (VS Code)

Create `.vscode/settings.json` with the following content:

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": "always",
    "source.organizeImports": "always"
  },
  "files.associations": {
    "*.css": "tailwindcss"
  },
  "typescript.tsdk": "node_modules/typescript/lib"
}
```

Commit the settings file.

#### 3️⃣ Integrate Prettier & ESLint

1. **Add ESLint‑Prettier bridge**

```bash
pnpm add -D eslint-config-prettier
```

2. **Extend the ESLint config** (`eslint.config.mjs`)

```js
import nextVitals from "eslint-config-next/core-web-vitals";
import prettier from "eslint-config-prettier/flat";
import { defineConfig, globalIgnores } from "eslint/config";

export default defineConfig([
  ...nextVitals,
  prettier,
  globalIgnores([".next/**", "out/**", "build/**", "next-env.d.ts"]),
]);
```

**Reference:** <https://nextjs.org/docs/app/api-reference/config/eslint#disabling-rules>  
**Reference:** <https://nextjs.org/docs/app/api-reference/config/eslint#with-prettier>

3. **Add sorting & Tailwind plugins**

```bash
pnpm add -D @trivago/prettier-plugin-sort-imports
pnpm add -D prettier prettier-plugin-tailwindcss
```

4. **Create `.prettierrc.json`**

```json
{
  "semi": true,
  "singleQuote": false,
  "tabWidth": 2,
  "trailingComma": "es5",
  "importOrder": [
    "^(react|next?/?([a-zA-Z/]*))$",
    "<THIRD_PARTY_MODULES>",
    "^@/(.*)$",
    "^[./]"
  ],
  "importOrderSeparation": true,
  "importOrderSortSpecifiers": true,
  "plugins": [
    "@trivago/prettier-plugin-sort-imports",
    "prettier-plugin-tailwindcss"
  ]
}
```

**Reference:** <https://github.com/trivago/prettier-plugin-sort-imports/>  
**Reference:** <https://tailwindcss.com/blog/automatic-class-sorting-with-prettier>

5. **Verify** – format a component with mixed Tailwind classes and run:

```bash
pnpm lint
```

#### 4️⃣ Add a format script

Add to `package.json`:

```json
{
  "scripts": {
    "format": "prettier src/ --write"
  }
}
```

Run `pnpm run format` to confirm all files are formatted.

#### 5️⃣ Install `eslint-plugin-check-file`

```bash
pnpm add -D eslint-plugin-check-file
```

Add the plugin to `eslint.config.mjs` (example snippet):

```js
import checkFile from "eslint-plugin-check-file";

export default defineConfig([
  // …previous config
  {
    plugins: { checkFile },
    rules: {
      // enable desired rules from the plugin
      "check-file/filename-match-export": "error",
    },
  },
]);
```

Run `pnpm lint` to ensure the rule is active.

**Reference:** <https://www.npmjs.com/package/eslint-plugin-check-file?activeTab=readme>  
**Reference:** <https://github.com/alan2207/bulletproof-react/blob/master/docs/project-standards.md>

#### 6️⃣ Enable typed routes

Edit `next.config.js` (or `next.config.mjs`) and add:

```js
module.exports = {
  // …other config
  typedRoutes: true,
};
```

Restart the dev server and verify TypeScript provides route typings.

**Reference:** <https://nextjs.org/docs/app/api-reference/config/typescript>

#### 7️⃣ Emoji favicon

In `app/layout.tsx` add the following inside `<head>`:

```tsx
<link
  rel="icon"
  href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>📦</text></svg>"
/>
```

Check that the emoji appears on the browser tab.

**Reference:** <https://css-tricks.com/emoji-as-a-favicon/>

#### 8️⃣ Export default viewport constant

```tsx
import { Viewport } from "next";

export const viewport: Viewport = {
  themeColor: "#ffffff",
  // add other viewport settings as needed
};
```

Use the exported `viewport` in your app without TypeScript errors.

**References:**

- <https://tailwindcss.com/docs/responsive-design>
- <https://nextjs.org/docs/app/api-reference/functions/generate-viewport>

---

### Quick Commands Summary

```bash
# Scaffold
pnpm create next-app@latest next-base
cd next-base
pnpm dev

# Install dev dependencies
pnpm add -D eslint-config-prettier @trivago/prettier-plugin-sort-imports prettier prettier-plugin-tailwindcss eslint-plugin-check-file

# Lint & format
pnpm lint
pnpm run format
```

Happy coding!
