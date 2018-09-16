---
title: Configure ESLint for a Sitecore JavaScript Services (JSS) React Project
tags: [Sitecore, JSS, Linting, ESLint, React]
sharing:
  linkedin: Check how to configure ESLint for a Sitecore JavaScript Services (JSS) React Project in my latest blog post.
---

Linting is important in a code project. Among many other things, it can ensure all the code is using the same formatting standards, there are no unused variables or unresolved imports.

Sitecore JSS projects can really benefit using a linter.

<!-- more -->

## Default Linting

The JSS sample React application is created using [`react-scripts`](https://www.npmjs.com/package/react-scripts) and [`create-react-app`](https://github.com/facebook/create-react-app). When running `jss start`, ESLint is used with a default configuration to execute basic validations like unresolved imports.

## ESLint Configuration File

The JSS sample React application comes with a more advanced `.eslintrc` file. However, it is not used when running `jss start`. It could be used with a globally installed ESLint and an ESLint extension in the text editor. However, the `.eslintrc` file references packages that are not installed when running `jss create` such as `eslint-plugin-prettier`, `eslint-plugin-import`, and `eslint-plugin-react`. This causes warnings when editing source files as the ESLint extension is unable to lint.

> Cannot find module 'eslint-plugin-prettier' Referenced from: C:\jss-react-app-root\.eslintrc

## Manually Running ESLint

The first step is to configure the project to lint on demand using a script.

> **NOTE:** `react-scripts` specifically requires ESLint version 4.19.1.

To install ESLint and all referenced packages locally:

```
yarn add eslint@4.19.1 eslint-plugin-import eslint-plugin-react eslint-plugin-prettier eslint-config-prettier -D
```
or
```
npm install eslint@4.19.1 eslint-plugin-import eslint-plugin-react eslint-plugin-prettier eslint-config-prettier --save-dev
```

Next, the script to run ESLint must be added to the `package.json` file:

```json
{
  "scripts": {
    "lint": "node .\\node_modules\\eslint\\bin\\eslint.js **/*.js"
  }
}
```

Finally, to run ESLint:

```
yarn run lint
```
or
```
npm run lint
```

On the first run, there are hundreds of errors.

```
✖ 788 problems (788 errors, 0 warnings)
  606 errors and 0 warnings potentially fixable with the `--fix` option.

error Command failed with exit code 1.
```

## Fixing Issues

Some ESLint errors can be automatically fixed in the codebase. This is the case of code formatting issues identified by Prettier.

To fix issues:

```
yarn run lint --fix
```
or
```
npm run lint --fix
```

After fixing, there are 182 errors remaining:

```
✖ 182 problems (182 errors, 0 warnings)

error Command failed with exit code 1.
```

Most of them are unresolved imports:

```
error  Unable to resolve path to module 'react'                             import/no-unresolved
```

Those are caused by a missing configuration in the `.eslintrc` file. The `import/resolver` plugin does not know how to resolve imports when running inside nodejs.

The solution is to add a setting to tell it to resolve the imports as JavaScript files:

```json
{
  "settings": {
    "import/resolver": {
      "node": {
        "extensions": ['.js']
      }
    }
  }
}
```

Running ESLint again gives only 1 error:

```
C:\jss-react-app-root\server\server.js
  11:27  error  Unable to resolve path to module '../build/index.html'  import/no-unresolved

✖ 1 problem (1 error, 0 warnings)

error Command failed with exit code 1.
```

This error is not important at the moment as this file gets created later in the development process when switching away from the code-first development mode.

## Using the Local ESLint Configuration in `jss start`

At this point, `jss start` is still using a default ESLint configuration instead of the local `.eslintrc` file. This is caused by `react-scripts`.

One option is to [eject](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#npm-run-eject) but it highly complexifies an application as it now have full control on everything `react-scripts` are doing under the cover.

Instead, to keep all the advantages of `react-scripts` but only take control of ESLint, a better option is to rewire only the ESLint portion using a simple override configuration. Two packages must be installed first:

```
yarn add react-app-rewired react-app-rewire-eslint
```
or
```
npm install react-app-rewired react-app-rewire-eslint
```

Then, a small `config-overrides.js` file must be created at the root of the application besides the `package.json` file:

```javascript
const rewireEslint = require("react-app-rewire-eslint");

module.exports = function override(config, env) {
  config = rewireEslint(config, env);
  return config;
};
```

Lastly, all the `package.json` scripts using `react-scripts` must be modified to instead use `react-app-rewired`:

```json
{
  "scripts": {
    "start:react": "react-app-rewired start",
    "build:client": "cross-env-shell PUBLIC_URL=$npm_package_config_sitecoreDistPath \"react-app-rewired build\"",
    "test": "react-app-rewired test --env=jsdom",
    "eject": "react-app-rewired eject"
  }
}
```

Now, the next time `jss start` is run, it is using the local ESLint configuration file. There is a warning about the react version that is not important:

> Warning: React version not specified in eslint-plugin-react settings. See https://github.com/yannickcr/eslint-plugin-react#configuration.

It can be removed by specifying the React version in the `.eslintrc` file. At the time of writing these lines, the version is 16.3.2:

```json
{
  "settings": {
    "react": {
      "version": "16.3.2"
    }
  }
}
```