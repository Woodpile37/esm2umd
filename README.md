esm2umd
=======

> Fork of [esm2umd](https://github.com/dcodeIO/esm2umd), namespaces under `@leichtgewicht`
> that transforms all .mjs files found in a folder to .cjs files and makes sure that they
> run in older node versions by replacing the package name inside the files.

Transforms ESM to UMD, i.e. to use ESM by default with UMD as a legacy fallback.

[![Build Status](https://img.shields.io/github/workflow/status/dcodeIO/esm2umd/Test/main?label=test&logo=github)](https://github.com/dcodeIO/esm2umd/actions?query=workflow%3ATest) [![Publish Status](https://img.shields.io/github/workflow/status/dcodeIO/esm2umd/Publish/main?label=publish&logo=github)](https://github.com/dcodeIO/esm2umd/actions?query=workflow%3APublish) [![npm](https://img.shields.io/npm/v/esm2umd.svg?label=npm&color=007acc&logo=npm)](https://www.npmjs.com/package/esm2umd)

Usage
-----

```
npx @leichtgewicht/esm2umd MyModule
```

`MyModule` is used as the name of the vanilla JS global.

If the module has a `default` export, it becomes the value obtained when `require`d.

API
---

Installation as a dependency is optional (pulls in megabytes of babel), but if so desired exposes the CLI as an API:

```js
import esm2umd from 'esm2umd'

const esmCode = '...'
const umdCode = esm2umd('ModuleName', esmCode)
```

Example
-------

ESM-first hybrid module with legacy fallback and prepublish build step.

**package.json**

```json
{
  "type": "module",
  "main": "./umd/index.js",
  "types": "index.d.ts",
  "exports": {
    "import": "./index.js",
    "require": "./umd/index.js"
  },
  "scripts": {
    "build": "npx esm2umd MyModule",
    "prepublishOnly": "npm run build"
  }
}
```

Treat .js files in `umd/` as CommonJS.

**umd/package.json**

```json
{
  "type": "commonjs"
}
```

Keep the generated artifact out of version control to avoid PRs against it.

**.gitignore**

```
umd/index.js
```

For typings, if there is a `default` export, stick to the "old" format for compatibility.

**index.d.ts**

```ts
export = MyModule;
```
