# Neutrino Vue Preset

[![NPM version][npm-image]][npm-url]
[![NPM downloads][npm-downloads]][npm-url]
[![Join the Neutrino community on Spectrum][spectrum-image]][spectrum-url]

## Features

- Zero upfront configuration necessary to start developing and building a Vue web app
- Modern Babel compilation.
- Extends from [@neutrinojs/web](https://neutrino.js.org/packages/web)
  - Modern Babel compilation supporting ES modules, last 2 major browser versions, async functions, and dynamic imports
  - Webpack loaders for importing HTML, CSS, images, icons, fonts, and web workers
  - Webpack Dev Server during development
  - Automatic creation of HTML pages, no templating necessary
  - Hot Module Replacement support
  - Tree-shaking to create smaller bundles
  - Production-optimized bundles with Babel minification, easy chunking, and scope-hoisted modules for faster execution
  - Easily extensible to customize your project as needed

## Requirements

- Node.js v6.10+
- Yarn or npm client
- Neutrino v7

## Installation

`@neutrinojs/vue` can be installed via the Yarn or npm clients. Inside your project, make sure
`neutrino` and `@neutrinojs/vue` are development dependencies. You will also need Vue for actual
Vue development.

#### Yarn

```bash
❯ yarn add --dev neutrino @neutrinojs/vue
❯ yarn add vue
```

#### npm

```bash
❯ npm install --save-dev neutrino @neutrinojs/vue
❯ npm install --save vue


## Project Layout

`@neutrinojs/vue` follows the standard [project layout](https://neutrino.js.org/project-layout) specified by Neutrino. This
means that by default all project source code should live in a directory named `src` in the root of the
project. This includes JavaScript files, CSS stylesheets, images, and any other assets that would be available
to import your compiled project.

## Quickstart

After installing Neutrino and the Vue preset, add a new directory named `src` in the root of the project, with
two files `index.js` and `App.vue` in it.

```bash
❯ mkdir src && touch src/index.js && touch src/App.vue
```

This Vue preset exposes an element in the page with an ID of `root` to which you can mount your application. Edit
your `src/index.js` file with the following:

```js
import Vue from 'vue';
import App from './App.vue';

new Vue({
  el: '#root',
  render: (h) => h(App),
});
```

Next, edit your `src/App.vue` with the following:

```html
<script>
  export default {
    name: 'App',
    data() {
      return {};
    }
  };
</script>

<template>
  <div>
    <h1>Hello world!</h1>
  </div>
</template>
```

Now edit your project's package.json to add commands for starting and building the application:

```json
{
  "scripts": {
    "start": "neutrino start --use @neutrinojs/vue",
    "build": "neutrino build --use @neutrinojs/vue"
  }
}
```

If you are using `.neutrinorc.js`, add this preset to your use array instead of `--use` flags:

```js
module.exports = {
  use: ['@neutrinojs/vue']
};
```

#### Yarn

```bash
❯ yarn start
✔ Development server running on: http://localhost:5000
✔ Build completed
```

#### npm

```bash
❯ npm start
✔ Development server running on: http://localhost:5000
✔ Build completed
```

Start the app, then open a browser to the address in the console:

## Building

`@neutrinojs/vue` builds static assets to the `build` directory by default when running `neutrino build`. Using
the quick start example above as a reference:

```bash
❯ yarn build

✔ Building project completed
Hash: b26ff013b5a2d5f7b824
Version: webpack 3.5.6
Time: 9773ms
                           Asset       Size    Chunks             Chunk Names
   index.dfbad882ab3d86bfd747.js     181 kB     index  [emitted]  index
 runtime.3d9f9d2453f192a2b10f.js    1.51 kB   runtime  [emitted]  runtime
                      index.html  846 bytes            [emitted]
✨  Done in 14.62s.
```

You can either serve or deploy the contents of this `build` directory as a static site.

## Static assets

If you wish to copy files to the build directory that are not imported from application code, you can place
them in a directory within `src` called `static`. All files in this directory will be copied from `src/static`
to `build/static`. To change this behavior, specify your own patterns with
[@neutrinojs/copy](https://neutrino.js.org/packages/copy).

## Paths

The `@neutrinojs/web` preset loads assets relative to the path of your application by setting Webpack's
[`output.publicPath`](https://webpack.js.org/configuration/output/#output-publicpath) to `./`. If you wish to load
assets instead from a CDN, or if you wish to change to an absolute path for your application, customize your build to
override `output.publicPath`. See the [Customizing](#Customizing) section below.


## Preset options

You can provide custom options and have them merged with this preset's default options to easily affect how this
preset builds. You can modify Vue preset settings from `.neutrinorc.js` by overriding with an options object. Use
an array pair instead of a string to supply these options in `.neutrinorc.js`.

The following shows how you can pass an options object to the Vue preset and override its options. See the
[Web documentation](https://neutrino.js.org/packages/web#preset-options) for specific options you can override with this object.

```js
module.exports = {
  use: [
    ['@neutrinojs/vue', {
      /* preset options */

      // Example: disable Hot Module Replacement
      hot: false,

      // Example: change the page title
      html: {
        title: 'Epic Vue App'
      },

      // Target specific browsers with babel-preset-env
      targets: {
        browsers: [
          'last 1 Chrome versions',
          'last 1 Firefox versions'
        ]
      },

      // Add additional Babel plugins, presets, or env options
      babel: {
        // Override options for babel-preset-env:
        presets: [
          ['babel-preset-env', {
            modules: false,
            useBuiltIns: true,
            exclude: ['transform-regenerator', 'transform-async-to-generator'],
          }]
        ]
      }
    }]
  ]
};
```

## Customizing

To override the build configuration, start with the documentation on [customization](https://neutrino.js.org/customization).
`@neutrinojs/vue` creates some conventions to make overriding the configuration easier once you are ready to make
changes. Most of the configuration for `@neutrinojs/vue` is inherited from the
[`@neutrinojs/web` preset](https://neutrino.js.org/packages/web#customizing); continue to that documentation
for details on customization.

By default Neutrino, and therefore this preset, creates a single **main** `index` entry point to your application, and
this maps to the `index.*` file in the `src` directory. The extension is resolved by webpack. This value is provided by
`neutrino.options.mains` at `neutrino.options.mains.index`. This means that the Web preset is optimized toward the use
case of single-page applications over multi-page applications. If you wish to output multiple pages, you can detail
all your mains in your `.neutrinorc.js`.
 
```js
module.exports = {
  options: {
    mains: {
      index: 'index', // outputs index.html from src/index.*
      admin: 'admin', // outputs admin.html from src/admin.*
      account: 'user' // outputs account.html from src/user.*
    }
  },
  use: ['@neutrinojs/vue']
}
```

### Rules

The following is a list of additional rules and their identifiers this preset defines, in addition
to the ones provided by `@neutrinojs/web`, which can be overridden:

| Name | Description | Environments and Commands |
| --- | --- | --- |
| `vue` | Compiles Vue files from the `src` directory using Babel and `vue-loader`. Contains a single loader named `vue`. | all |

### Plugins

_This preset does not define any additional plugins from those already in use by `@neutrinojs/web`._

### Advanced configuration

By following the [customization guide](https://neutrino.js.org/customization) and knowing the rule, loader, and plugin IDs from
`@neutrinojs/web` and above, you can override and augment the build by providing a function to your `.neutrinorc.js` use
array. You can also make these changes from the Neutrino API in custom middleware.

#### Vendoring

By defining an entry point named `vendor` you can split out external dependencies into a chunk separate
from your application code.

_Example: Put Vue into a separate "vendor" chunk:_

```js
module.exports = {
  use: [
    '@neutrinojs/vue',
    (neutrino) => {
      neutrino.config
        .entry('vendor')
          .add('vue');
    }
  ]
};
```

## Contributing

This preset is part of the [neutrino-dev](https://github.com/mozilla-neutrino/neutrino-dev) repository, a monorepo
containing all resources for developing Neutrino and its core presets and middleware. Follow the
[contributing guide](https://neutrino.js.org/contributing) for details.

[npm-image]: https://img.shields.io/npm/v/@neutrinojs/vue.svg
[npm-downloads]: https://img.shields.io/npm/dt/@neutrinojs/vue.svg
[npm-url]: https://npmjs.org/package/@neutrinojs/vue
[spectrum-image]: https://withspectrum.github.io/badge/badge.svg
[spectrum-url]: https://spectrum.chat/neutrino
