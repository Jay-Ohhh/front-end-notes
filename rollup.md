### rollup 和 webpack

**在开发应用时使用 Webpack，开发库时使用 Rollup**

rollup的定位是偏向于JavaScript库的构建，打包的体积也比webpack的小。

Rollup有一项webpack不具有的功能：通过配置output.format开发者可以选择输出资源的模块形式（cjs（CommonJS），esm（ES6 Modules），amd，umd，iife，system等），此特性对于打包JavaScript库特别有用，因为往往一个库需要支持多种不同的模块形式，而通过rollup几个命令就可以把一份源代码打包为多份。

### 使用TypeScript编写配置文件

```sh
npm i -D rollup-plugin-typescript2
```

项目根目录添加 `tsconfig.json`

**rollup.config.ts**

```ts
import { RollupOptions } from 'rollup';

const isProd = process.env.NODE_ENV === 'production';

const options: RollupOptions[] = [
  {
    input: 'src/index.ts',
    output: [
      {
        file: 'dist/index.js',
        format: 'cjs',
      },
      {
        file: 'dist/index.esm.js',
        format: 'esm',
      },
    ],
  },
];

export default options;
```

```sh
rollup --config rollup.config.ts --configPlugin typescript2
```

### 配置选项

```js
// rollup.config.js

// can be an array (for multiple inputs)
export default {
  // core input options
  external,
  input, // conditionally required
  plugins,

  // advanced input options
  cache,
  onwarn,
  preserveEntrySignatures,
  strictDeprecations,

  // danger zone
  acorn,
  acornInjectPlugins,
  context,
  moduleContext,
  preserveSymlinks,
  shimMissingExports,
  treeshake,

  // experimental
  experimentalCacheExpiry,
  perf,

  // required (can be an array, for multiple outputs)
  output: {
    // core output options
    dir,
    file,
    format, // required
    globals,
    name,
    plugins,

    // advanced output options
    assetFileNames,
    banner,
    chunkFileNames,
    compact,
    entryFileNames,
    extend,
    footer,
    hoistTransitiveImports,
    inlineDynamicImports,
    interop,
    intro,
    manualChunks,
    minifyInternalExports,
    outro,
    paths,
    preserveModules,
    preserveModulesRoot,
    sourcemap,
    sourcemapExcludeSources,
    sourcemapFile,
    sourcemapPathTransform,
    validate,

    // danger zone
    amd,
    esModule,
    exports,
    externalLiveBindings,
    freeze,
    indent,
    namespaceToStringTag,
    noConflict,
    preferConst,
    sanitizeFileName,
    strict,
    systemNullSetters
  },

  watch: {
    buildDelay,
    chokidar,
    clearScreen,
    skipWrite,
    exclude,
    include
  }
};
```

#### input

Type: `string | string [] | { [entryName: string]: string }`
CLI: `-i`/`--input <filename>`

入口文件路径

多入口：

除非使用了 `output.file`，否则遵循 `ouput.entryFileNames`。

如果使用对象形式，则 [name] 的值对应为对象的 key

如果使用数组形式，则[name] 的值对应为每个数组元素

```js
// rollup.config.js
export default {
  ...,
  input: {
    a: 'src/main-a.js',
    'b/index': 'src/main-b.js'
  },
  // or
  // input:['src/main-a.js', 'src/main-b.js'],
  output: {
    ...,
    entryFileNames: 'entry-[name].js'
  }
};
```

或

```js
// rollup.config.js (building more than one bundle)

export default [
  {
    input: 'main-a.js',
    output: {
      file: 'dist/bundle-a.js',
      format: 'cjs'
    }
  },
  {
    input: 'main-b.js',
    output: [
      {
        file: 'dist/bundle-b1.js',
        format: 'cjs'
      },
      {
        file: 'dist/bundle-b2.js',
        format: 'es'
      }
    ]
  }
];
```

#### output

##### output.dir

Type: `string`
CLI: `-d`/`--dir <dirname>`

放置生成的多个chunk的目录。生成多个chunk（超过一个）则使用该选项，否则使用`file`选项。

生成多个chunk的情况：

- input选项为多入口
- 动态 import 会进行 code-splitting，而且`UMD and IIFE output formats are not supported for code-splitting builds`。

##### output.file

Type: `string`
CLI: `-o`/`--file <filename>`

`String` 要写入的文件，也可以用于生成 sourcemaps。只能在生成的chunk不超过一个时使用。

```js
// rollup.config.js
export default {
  ...,
  output: {
    file: 'lib/bundle.js',
    format: 'iife',
    name: 'MyBundle'
  }
};
```

##### output.format

Type: `string`
CLI: `-f`/`--format <formatspecifier>`
Default: `"es"`

生成包的格式。 与 webpack `output.library.type` 类似

- `amd` – 异步模块定义，用于像RequireJS这样的模块加载器
- `cjs` – CommonJS，适用于 Node 和 Browserify/Webpack。(alias: `commonjs`)
- `es` – 将软件包保存为 ES 模块文件，在现代浏览器中可以通过 `<script type=module>` 标签引入。 (alias: `esm`, `module`)
- `iife` – 一个自动执行的功能，适合作为`<script>`标签。（如果要为应用程序创建一个捆绑包，您可能想要使用它，因为它会使文件大小变小。）
- `umd` – 通用模块定义，以`amd`，`cjs` 和 `iife` 为一体
- `system` - SystemJS 加载器格式。alias: `systemjs`)

##### output.exports

Type: `string`
CLI: `--exports <exportMode>`
Default: `'auto'`

- `'auto'`  - 自动识别以下的export方式

- `default` – if you are only exporting one thing using `export default ...`; 

- `named` – 如果使用 named export , e.g. 
  
  ```js
  export const a = 1;
  ```

- `none` – if you are not exporting anything (e.g. you are building an app, not a library)

如果 format 为 cjs

- 入口文件仅使用 default export ，则需要**显式** `output.exports: 'auto' 或 'default'`

- 入口文件同时使用 named and default export，则需要显式 `output.exports: 'named'`
  
  If you use `default`, a CommonJS user could do this, for example:

```js
// your-lib package entry
export default 'Hello world';

// a CommonJS consumer
/* require( "your-lib" ) returns "Hello World" */
const hello = require('your-lib');
```

With `named`, a user would do this instead:

```js
// your-lib package entry
export const hello = 'Hello world';

// a CommonJS consumer
/* require( "your-lib" ) returns {hello: "Hello World"} */
const hello = require('your-lib').hello;
/* or using destructuring */
const { hello } = require('your-lib');
```

If you use `named` exports but *also* have a `default` export, a user would have to do something like this to use the default export:

```js
// your-lib package entry
export default 'foo';
export const bar = 'bar';

// a CommonJS consumer
/* require( "your-lib" ) returns {default: "foo", bar: "bar"} */
const foo = require('your-lib').default;
const bar = require('your-lib').bar;
/* or using destructuring */
const { default: foo, bar } = require('your-lib');
```

##### output.name

Type: `string`
CLI: `-n`/`--name <variableName>`

若生成包的格式是 `iife`/`umd`，则该选项是必须的，表示全局变量名。

```js
// rollup.config.js
export default {
  ...,
  output: {
    file: 'bundle.js',
    format: 'iife',
    name: 'MyBundle'
  }
};

// -> var MyBundle = (function () {...
```

##### output.entryFileNames

Type: `string | ((chunkInfo: ChunkInfo) => string)`
CLI: `--entryFileNames <pattern>`
Default: `"[name].js"`

The pattern to use for chunks created from entry points, or a function that is called per entry chunk to return such a pattern. Patterns support the following placeholders:

- `[format]`: The rendering format defined in the output options, e.g. `es` or `cjs`.
- `[hash]`: A hash based on the content of the entry point and the content of all its dependencies.
- `[name]`: The file name (without extension) of the entry point, unless the object form of input was used to define a different name.

##### output.chunkFileNames

Type: `string | ((chunkInfo: ChunkInfo) => string)`
CLI: `--chunkFileNames <pattern>`
Default: `"[name]-[hash].js"`

The pattern to use for naming shared chunks created when code-splitting, or a function that is called per chunk to return such a pattern. Patterns support the following placeholders:

- `[format]`: The rendering format defined in the output options, e.g. `es` or `cjs`.
- `[hash]`: A hash based on the content of the chunk and the content of all its dependencies.
- `[name]`: The name of the chunk. This can be explicitly set via the [`output.manualChunks`](https://rollupjs.org/guide/en/#outputmanualchunks) option or when the chunk is created by a plugin via [`this.emitFile`](https://rollupjs.org/guide/en/#thisemitfile). Otherwise, it will be derived from the chunk contents.

##### output.plugins

Type: `OutputPlugin | (OutputPlugin | void)[]`

与 plugins 选项类似。

不是所有的插件都能在该选项中使用。只有在`bundle.generate()` or `bundle.write()`，即`Rollup's main analysis`完成后使用。

If you are a plugin author, see [output generation hooks](https://rollupjs.org/guide/en/#output-generation-hooks) to find out which hooks can be used.

##### output.globals

Type: `{ [id: string]: string } | ((id: string) => string)`
CLI: `-g`/`--globals <external-id:variableName,another-external-id:anotherVariableName,...>`

`Object` 形式的 `moduleId/variableName` 键值对

当你创建 `umd` 或 `iife` 包时，你必须在 `ouput.globals` 设置 `external`中包对应的全局变量名，例如

```js
import jq from 'jquery'; 
```

...我们想告诉 Rollup `jquery` 模块的id等同于 `$` 变量:

```js
// rollup.config.js
export default {
  ...,
  external: ['jquery'],
  output: {
    format: 'iife',
    name: 'MyBundle',
    globals: {
      jquery: '$' // jquery 是 moduleId，$ 是 全局变量名variableName
    }
  }
};

/*
var MyBundle = (function ($) {
  // code goes here
}($));
*/
```

当作为命令行参数给出时，它应该是一个逗号分隔的“id：name”键值对列表：

```bash
rollup -i src/main.js ... -g jquery:$,underscore:_
```

即使是导入库的某些变量，globals也是这样写

```js
import { Link } from "react-router-dom";
```

```js
// rollup.config.js
globals: {
    'react-router-dom': 'ReactRouterDOM'
}
```

##### output.paths

Type: `{ [id: string]: string } | ((id: string) => string)`

Paths supplied by `output.paths` will be used in the generated bundle instead of the module ID, allowing you to, for example, load dependencies from a CDN:

```js
// app.js
import { selectAll } from 'd3';
selectAll('p').style('color', 'purple');
// ...

// rollup.config.js
export default {
  input: 'app.js',
  external: ['d3'],
  output: {
    file: 'bundle.js',
    format: 'amd',
    paths: {
      d3: 'https://d3js.org/d3.v4.min'
    }
  }
};

// bundle.js
define(['https://d3js.org/d3.v4.min'], function (d3) {
  d3.selectAll('p').style('color', 'purple');
  // ...
});
```

##### output.banner/output.footer

Type: `string | (() => string | Promise<string>)`
CLI: `--banner`/`--footer <text>`

添加 string 到文件头部或底部。

Note: `banner` and `footer` options will not break sourcemaps.

```js
// rollup.config.js
export default {
  ...,
  output: {
    ...,
    banner: '/* my-library version ' + version + ' */',
    footer: '/* follow me on Twitter! @rich_harris */'
  }
};
```

##### output.intro/output.outro

Type: `string | (() => string | Promise<string>)`
CLI: `--intro`/`--outro <text>`

与 **output.banner/output.footer** 类似

在文件顶部或底部运行配置的代码

```js
export default {
  ...,
  output: {
    ...,
    intro: 'const ENVIRONMENT = "production";'
  }
};
```

##### output.sourcemap

Type: `boolean | 'inline' | 'hidden'`
CLI: `-m`/`--sourcemap`/`--no-sourcemap`
Default: `false`

如果为true，将创建一个单独的sourcemap文件。如果是“inline”，则sourcemap将作为 data URI 附加到结果输出文件中。“hidden”与true类似，只是打包文件中相应的sourcemap注释被抑制。

##### output.sourcemapExcludeSources

Type: `boolean`
CLI: `--sourcemapExcludeSources`/`--no-sourcemapExcludeSources`
Default: `false`

如果为true，源代码的实际代码将不会添加到sourcemaps中，从而使它们变得更小。

##### output.sourcemapFile

Type: `string`
CLI: `--sourcemapFile <file-name-with-path>`

存放sourcemap文件的目录。

如果指定了output，则ourcemapFile不是必需的，在这种情况下，会在打包输出文件的目录下生成`.map`文件。

#### external

Type: `(string | RegExp)[] | RegExp | string | (id: string, parentId: string, isResolved: boolean) => boolean`
CLI: `-e`/`--external <external-id,another-external-id,...>`

external 声明外部依赖，可以不被打包

 `Array` 应该保留在bundle的外部引用的模块ID。ID应该是：

1. 外部依赖的名称
2. 一个已被找到路径的ID（像文件的绝对路径）

```js
// rollup.config.js
import path from 'path';

export default {
  ...,
  external: [
    'some-externally-required-library',
    path.resolve( './src/some-local-file-that-should-not-be-bundled.js' )
  ]
};
```

当作为命令行参数给出时，它应该是以逗号分隔的ID列表：

```bash
rollup -i src/main.js ... -e foo,bar,baz
```

当你创建 `umd` 或 `iife` 包时，你必须在 `ouput.globals` 设置 `external`中包对应的全局变量名，而例如

```js
{
    input: 'src/index.ts',
  output: [
      {
          file: pkg.module,
          format: 'umd',
          name: "xxx",
          globals:{
              react: "React",
        "react-dom": "ReactDOM",
              jquery: "$",
          }
      },
  ],
  external:['react', 'react-dom', 'jquery']
}
```

#### external、output.globals、output.paths、peerDependencies

external 通常和 output.globals、output.paths 搭配使用。与 webpack 的 externals 选项类似。

peerDependencies的包通常可以通过 external 从打包中剔除。

**rollup.config.js**

```js
import pkg from './package.json';

export default {
    external: Object.keys(pkg.peerDependencies || {})
}
```

#### plugins

Type: `Plugin | (Plugin | void)[]`

有关详细信息请参阅 [插件入门](https://rollupjs.org/guide/en/#using-plugins)。记住要调用导入的插件函数(即 `commonjs()`, 而不是 `commonjs`).

```js
// rollup.config.js
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';

const isProduction = process.env.NODE_ENV === 'production';

export default (async () => ({
  input: 'main.js',
  plugins: [resolve(), commonjs(), isProduction && (await import('rollup-plugin-terser')).terser()],
  output: {
    file: 'bundle.js',
    format: 'cjs'
  }
}))();
```

#### onwarn

Type: `(warning: RollupWarning, defaultHandler: (warning: string | RollupWarning) => void) => void;`

警告消息拦截器。如果未提供，警告将被删除并打印到控制台。使用--silent CLI（不要将警告打印到控制台）选项时，此处理程序是获得警告通知的唯一方法。

该方法接受两个参数：warning object 和 默认的处理方法. 

Warnings objects 至少 `code`  、`message` 两个属性， 这意味着您可以控制如何处理不同类型的警告。

其余属性则是根据警告类型添加。

```js
// rollup.config.js
export default {
  ...,
  onwarn (warning, warn) {
    // skip certain warnings
    if (warning.code === 'UNUSED_EXTERNAL_IMPORT') return;

    // throw on others
    if (warning.code === 'NON_EXISTENT_EXPORT') throw new Error(warning.message);

    // Use default for everything else
    warn(warning);
  }
};
```

许多warnings包含`loc`属性和一个`frame`，你可以定位到警告的来源：

```js
// rollup.config.js
export default {
  ...,
  onwarn ({ loc, frame, message }) {
    if (loc) {
      console.warn(`${loc.file} (${loc.line}:${loc.column}) ${message}`);
      if (frame) console.warn(frame);
    } else {
      console.warn(message);
    }
  }
};
```

#### watch

Type: `{ buildDelay?: number, chokidar?: ChokidarOptions, clearScreen?: boolean, exclude?: string, include?: string, skipWrite?: boolean } | false`
Default: `{}`

仅在运行 Rollup 时使用 `--watch` 标志或使用 `rollup.watch` 时生效。

##### watch.clearScreen

Type: `boolean`
CLI: `--watch.clearScreen`/`--no-watch.clearScreen`
Default: `true`

Whether to clear the screen when a rebuild is triggered.

##### watch.exclude

Type: `string | RegExp | (string | RegExp)[]`
CLI: `--watch.exclude <files>`

Prevent files from being watched:

```js
// rollup.config.js
export default {
  ...,
  watch: {
    exclude: 'node_modules/**'
  }
};
```

##### watch.include

Type: `string | RegExp | (string | RegExp)[]`
CLI: `--watch.include <files>`

Limit the file-watching to certain files. 

```js
// rollup.config.js
export default {
  ...,
  watch: {
    include: 'src/**'
  }
};
```

### 命令行参数

```
-c, --config <filename>     Use this config file (if argument is used but value
                              is unspecified, defaults to rollup.config.js)
--configPlugin              Allows specifying Rollup plugins to transpile or otherwise control the parsing                               of your configuration file.           
-d, --dir <dirname>         Directory for chunks (if absent, prints to stdout)
-e, --external <ids>        Comma-separate list of module IDs to exclude
-f, --format <format>       Type of output (amd, cjs, es, iife, umd, system)
-g, --globals <pairs>       Comma-separate list of `moduleID:Global` pairs
-h, --help                  Show this help message
-i, --input <filename>      Input (alternative to <entry file>)
-m, --sourcemap             Generate sourcemap (`-m inline` for inline map)
-n, --name <name>           Name for UMD export
-o, --file <output>         Single output file (if absent, prints to stdout)
-p, --plugin <plugin>       Use the plugin specified (may be repeated)
-v, --version               Show version number
-w, --watch                 Watch files in bundle and rebuild on changes
--amd.id <id>               ID for AMD module (default is anonymous)
--amd.autoId                Generate the AMD ID based off the chunk name
--amd.basePath <prefix>     Path to prepend to auto generated AMD ID
--amd.define <name>         Function to use in place of `define`
--assetFileNames <pattern>  Name pattern for emitted assets
--banner <text>             Code to insert at top of bundle (outside wrapper)
--chunkFileNames <pattern>  Name pattern for emitted secondary chunks
--compact                   Minify wrapper code
--context <variable>        Specify top-level `this` value
--entryFileNames <pattern>  Name pattern for emitted entry chunks
--environment <values>      Settings passed to config file (see example)
--no-esModule               Do not add __esModule property
--exports <mode>            Specify export mode (auto, default, named, none)
--extend                    Extend global variable defined by --name
--no-externalLiveBindings   Do not generate code to support live bindings
--failAfterWarnings         Exit with an error if the build produced warnings
--footer <text>             Code to insert at end of bundle (outside wrapper)
--no-freeze                 Do not freeze namespace objects
--no-hoistTransitiveImports Do not hoist transitive imports into entry chunks
--no-indent                 Don't indent result
--no-interop                Do not include interop block
--inlineDynamicImports      Create single bundle when using dynamic imports
--intro <text>              Code to insert at top of bundle (inside wrapper)
--minifyInternalExports     Force or disable minification of internal exports
--namespaceToStringTag      Create proper `.toString` methods for namespaces
--noConflict                Generate a noConflict method for UMD globals
--outro <text>              Code to insert at end of bundle (inside wrapper)
--preferConst               Use `const` instead of `var` for exports
--no-preserveEntrySignatures Avoid facade chunks for entry points
--preserveModules           Preserve module structure
--preserveModulesRoot       Put preserved modules under this path at root level
--preserveSymlinks          Do not follow symlinks when resolving files
--no-sanitizeFileName       Do not replace invalid characters in file names
--shimMissingExports        Create shim variables for missing exports
--silent                    Don't print warnings
--sourcemapExcludeSources   Do not include source code in source maps
--sourcemapFile <file>      Specify bundle position for source maps
--stdin=ext                 Specify file extension used for stdin input
--no-stdin                  Do not read "-" from stdin
--no-strict                 Don't emit `"use strict";` in the generated modules
--strictDeprecations        Throw errors for deprecated features
--systemNullSetters         Replace empty SystemJS setters with `null`
--no-treeshake              Disable tree-shaking optimisations
--no-treeshake.annotations  Ignore pure call annotations
--no-treeshake.moduleSideEffects Assume modules have no side-effects
--no-treeshake.propertyReadSideEffects Ignore property access side-effects
--no-treeshake.tryCatchDeoptimization Do not turn off try-catch-tree-shaking
--no-treeshake.unknownGlobalSideEffects Assume unknown globals do not throw
--waitForBundleInput        Wait for bundle input files
--watch.buildDelay <number> Throttle watch rebuilds
--no-watch.clearScreen      Do not clear the screen when rebuilding
--watch.skipWrite           Do not write files to disk when watching
--watch.exclude <files>     Exclude files from being watched
--watch.include <files>     Limit watching to specified files
--validate                  Validate output
```

`rollup -c` 默认使用 `rollup.config.js`，若使用其他配置文件

```sh
rollup --config rollup.config.dev.js
rollup --config rollup.config.prod.js
```

### 插件

#### 插件大全

https://github.com/rollup/awesome

| 插件名                                                                                               | 描述                                                                                        |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [alias](https://github.com/rollup/plugins/blob/master/packages/alias)                             | Define and resolve aliases for bundle dependencies 路径起别名                                  |
| [auto-install](https://github.com/rollup/plugins/blob/master/packages/auto-install)               | Automatically install dependencies that are imported by a bundle                          |
| [babel](https://github.com/rollup/plugins/blob/master/packages/babel)                             | Compile your files with Babel                                                             |
| [beep](https://github.com/rollup/plugins/blob/master/packages/beep)                               | System beeps on errors and warnings                                                       |
| [buble](https://github.com/rollup/plugins/blob/master/packages/buble)                             | Compile ES2015 with buble                                                                 |
| [commonjs](https://github.com/rollup/plugins/blob/master/packages/commonjs)                       | 将 CommonJS 转换成 ESM                                                                        |
| [clear](https://github.com/DongShelton/rollup-plugin-clear)                                       | Clear an output directory before a build.                                                 |
| [copy](https://github.com/meuter/rollup-plugin-copy)                                              | Copy files during a build.                                                                |
| [data-uri](https://github.com/rollup/plugins/blob/master/packages/data-uri)                       | Import modules from Data URIs                                                             |
| [dsv](https://github.com/rollup/plugins/blob/master/packages/dsv)                                 | Convert .csv and .tsv files into JavaScript modules with d3-dsv                           |
| [dynamic-import-vars](https://github.com/rollup/plugins/blob/master/packages/dynamic-import-vars) | Resolving dynamic imports that contain variables.                                         |
| [eslint](https://github.com/rollup/plugins/blob/master/packages/eslint)                           | Verify entry point and all imported files with ESLint                                     |
| [graphql](https://github.com/rollup/plugins/blob/master/packages/graphql)                         | Convert .gql/.graphql files to ES6 modules                                                |
| [html](https://github.com/rollup/plugins/blob/master/packages/html)                               | Create HTML files to serve Rollup bundles                                                 |
| [image](https://github.com/rollup/plugins/blob/master/packages/image)                             | Import JPG, PNG, GIF, SVG, and WebP files                                                 |
| [inject](https://github.com/rollup/plugins/blob/master/packages/inject)                           | Scan modules for global variables and injects `import` statements where necessary         |
| [json](https://github.com/rollup/plugins/blob/master/packages/json)                               | Convert .json files to ES6 modules                                                        |
| [legacy](https://github.com/rollup/plugins/blob/master/packages/legacy)                           | Add `export` declarations to legacy non-module scripts                                    |
| [multi-entry](https://github.com/rollup/plugins/blob/master/packages/multi-entry)                 | Use multiple entry points for a bundle                                                    |
| [node-resolve](https://github.com/rollup/plugins/blob/master/packages/node-resolve)               | 从node_modules中查找并打包第三方库                                                                   |
| [replace](https://github.com/rollup/plugins/blob/master/packages/replace)                         | Replace strings in files while bundling  替换字符串，与webpack.DefinePlugin相同                    |
| [run](https://github.com/rollup/plugins/blob/master/packages/run)                                 | Run your bundles in Node once they're built                                               |
| [strip](https://github.com/rollup/plugins/blob/master/packages/strip)                             | Remove debugger statements and functions like assert.equal and console.log from your code |
| [sucrase](https://github.com/rollup/plugins/blob/master/packages/sucrase)                         | Compile TypeScript, Flow, JSX, etc with Sucrase                                           |
| [terser](https://github.com/TrySound/rollup-plugin-terser)                                        | Minify a bundle using Terser                                                              |
| [typescript](https://github.com/rollup/plugins/blob/master/packages/typescript)                   | Integration between Rollup and Typescript                                                 |
| [url](https://github.com/rollup/plugins/blob/master/packages/url)                                 | Import files as data-URIs or ES Modules                                                   |
| [virtual](https://github.com/rollup/plugins/blob/master/packages/virtual)                         | Load virtual modules from memory                                                          |
| [wasm](https://github.com/rollup/plugins/blob/master/packages/wasm)                               | Import WebAssembly code with Rollup                                                       |
| [yaml](https://github.com/rollup/plugins/blob/master/packages/yaml)                               | Convert YAML files to ES6 modules                                                         |
| [progress](https://github.com/jkuri/rollup-plugin-progress)                                       | Show build progress in the console.                                                       |

### 常用插件

#### 常用配置

```shell
npm i -D rollup rollup-plugin-typescript2 @rollup/plugin-babel @rollup/plugin-node-resolve @rollup/plugin-commonjs rollup-plugin-terser rollup-plugin-clear rollup-plugin-progress rollup-plugin-visualizer rollup-plugin-copy cross-env
```

```ts
import { RollupOptions } from 'rollup';
import typescript from 'rollup-plugin-typescript2';
import babel from '@rollup/plugin-babel';
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';
import { terser } from 'rollup-plugin-terser';
import clear from 'rollup-plugin-clear';
import progress from 'rollup-plugin-progress';
import { visualizer } from 'rollup-plugin-visualizer';
import copy from 'rollup-plugin-copy';
import json from '@rollup/plugin-json';
import { DEFAULT_EXTENSIONS } from '@babel/core';
import pkg from './package.json';

const isProd = process.env.NODE_ENV === 'production';

const options: RollupOptions[] = [
  {
    input: 'src/index.ts',
    output: [
      {
        file: pkg.main,
        format: 'cjs',
        exports: 'auto',
        plugins: [
          // put it the last one
          isProd &&
            visualizer({
              filename: './out/report-cjs.html',
              open: true,
              gzipSize: true,
            }),
        ],
      },
      {
        file: pkg.module,
        format: 'es',
        plugins: [
          // put it the last one
          isProd &&
            visualizer({
              filename: './out/report-esm.html',
              open: true,
              gzipSize: true,
            }),
        ],
      },
    ],
    plugins: [
      clear({
        // required, point out which directories should be clear.
        targets: ['dist', 'out'], // 清空 out，否则visualizer会打开之前已存在的html
        // optional, whether clear the directores when rollup recompile on --watch mode.
        watch: true, // default: false
      }),
      progress({
        clearLine: true, // default: true
      }),
      copy({
        targets: [],
      }),
      json(),
      resolve({
        // default:true，需要显式设置来避免warning
        // true: 同名模块优先选择内置模块，例如（fs、path）
        // false: 同名模块优先选择安装的模块
        preferBuiltins: true,
      }),
      commonjs(),
      typescript(),
      babel({
        include: 'src/**/*',
        exclude: '**/node_modules/**',
        babelHelpers: 'runtime', // 构建库时使用
        extensions: [...DEFAULT_EXTENSIONS, '.ts', '.tsx'],
      }),
      isProd &&
        terser({
          compress: {
            // https://github.com/terser/terser
            // 默认会去掉debugger
            // drop_console: true, // 去掉console
            // pure_funcs:['console.log'] // 如果只想去除console.log，而不想去除console.info
          },
        }),
    ],
    // 注意 outpput.globals 也要添加对应的值
    external: [
      // @ts-ignore
      ...Object.keys(pkg.dependencies || {}),
      // @ts-ignore
      ...Object.keys(pkg.peerDependencies || {}),
      /@babel\/runtime/,
    ], //  babelHelpers:'runtime' 使用
  },
];

export default options;
```

#### TypeScript

```shell
npm install -D rollup cross-env
```

```shell
npm install -D  typescript tslib @types/node ts-node rollup-plugin-typescript2 nodemon
```

- ts-node —— 可选安装，在node.js上执行ts文件，可以方便调试ts文件。使用rollup的话，基本上是靠ts-node来调试。
  
  ```sh
  # Execute a script as `node` + `tsc`.
  ts-node script.ts
  ```

- tslib —— TypeScript的运行库，包含所有TypeScript帮助函数。rollup-plugin-typescript2 的依赖。
  
  > ```
  > // tsconfig.json
  > // importHelpers：Allow importing helper functions from tslib once per project, instead of including them per-file.
  > // 使用该属性时也需安装 tslib 
  > {
  >     "compilerOptions": {
  >         "importHelpers": true
  >     }
  > }
  > ```

**tsconfig.json**

```json
{
  "compilerOptions": {
    "module": "esnext",
    "target": "es5",
    "allowSyntheticDefaultImports": true /* 用来指定允许从没有默认导出的模块中默认导入 */,
    "jsx": "react",
    "lib": ["dom", "dom.iterable", "esnext"] /* 编译过程中需要引入的库文件的列表 */,
    "strict": true,
    "moduleResolution": "node",
    "experimentalDecorators": true /* 用于指定是否启用实验性的装饰器特性 */,
    "downlevelIteration": true /* 当target为'ES5' or 'ES3'时，为'for-of', spread, and destructuring'中的迭代器提供完全支持 */,
    "allowJs": true,
    "esModuleInterop": true /* 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性，esModuleInterop选项的作用是支持使用import d from 'cjs'的方式引入commonjs包 */,
    "resolveJsonModule": true,
    "forceConsistentCasingInFileNames": true /*     禁止对同一个文件的不一致的引用。 */,
    "noEmit": true /* 不生成编译文件 */,
    "noFallthroughCasesInSwitch": true /* 用于检查switch中是否有case没有使用break跳出switch，默认为false */,
    "noImplicitAny": false /* noImplicitAny的值为false时，如果我们没有为一些值设置明确的类型，编译器会默认认为这个值为any，如果noImplicitAny的值为true的话。则没有明确的类型会报错。默认值为false */,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    // 生成.d.ts文件
    "declaration": true,
    "declarationDir": "./types"
  },
  "include": ["src"],
  "exclude": ["**/node_modules/**", "build", "dist", "out"]
}
```

> "emitDeclarationOnly": false /* 默认为false，只生成.d.ts文件，不生成js文件，不能和 noEmit 同时设置为true */,
> 
> 不能设置为true，否则不会生成js文件，无法打包。

**rollup.config.ts**

```ts
import { RollupOptions } from 'rollup'
import typescript from 'rollup-plugin-typescript2'

const options: RollupOptions = {
  input: 'src/index.ts',
  output: [
    {
      file: 'dist/index.js',
      format: 'cjs',
    },
    {
      file: 'dist/index.esm.js',
      format: 'esm',
    },
  ],
  plugins: [
    typescript({
      // 默认为false，输出声明文件的路径为output.file或output.dir的路径
      // useTsconfigDeclarationDir: true, // 使用tsconfig.json的declarationDir，同时declaration要为true。
    })
  ],
}

export default options
```

**package.json**

```json
{
  "main": "dist/index.js",
  "module": "dist/index.esm.js",
  "typings": "dist/index.d.ts",
  "scripts": {
    "start": "cross-env NODE_ENV=development rollup --config rollup.config.ts --configPlugin typescript"
  }
}
```

#### node-resolve、commonjs

```sh
npm i -D @rollup/plugin-node-resolve @rollup/plugin-commonjs
```

- `@rollup/plugin-node-resolve`：从node_modules中查找并打包第三方库

- `@rollup/plugin-commonjs`：将 CommonJS 转换成 ESM
  
  > 当commonjs() 必须放在 babel() 前面

#### babel

```sh
npm i -D @babel/core @babel/preset-env @babel/preset-typescript @babel/preset-react @babel/plugin-transform-runtime @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties @rollup/plugin-babel
npm i @babel/runtime-corejs3 
```

**rollup.config.js**

**当使用 @rollup/rollup-plugin-commonjs 时，commonjs() 必须放在 babel() 前面**

`babelHelpers`

Type: `'bundled' | 'runtime' | 'inline' | 'external'`
Default: `'bundled'`

**'runtime'** - 如果你要用rollup构建一个js库的时候，使用该配置，该配置要结合@babel/plugin-transform-runtime插件使用，使用@babel/plugin-transform-runtime也要安装@babel/runtime 或 @babel/runtime-corejs2/3 插件。同时设置 `external: [/@babel\/runtime/]`，但被其他项目引入该库时，要确保已安装@babel/runtime 或 @babel/runtime-corejs2/3，即需要在package.json的dependencies或peerDependencies中添加依赖。

> 编译后的代码，会使用require('@babel/runtime')替换helper函数 ，例如编译promise：
> 
> ```
> var _Promise = require('@babel/runtime-corejs3/core-js/promise');
> ```

**'bundled'** - 如果用rollup构建一个项目应该用此参数。如果希望输出的文件包含helpers函数（最多复制一份），应该用此参数。此时babel不能使用替换helper的插件，例如 `@babel/plugin-transform-runtime`。

> 编译后的代码值会直接包含helper函数，不会包含运行时代码，即不会使用require('@babel/runtime')替换helper函数

**'external'**  要结合@babel/plugin-external-helpers插件使用，它会把helpers 收集到一个全局的共享模块。

**'inline'** 官网不推荐使用，会导致很多重复性代码

```js
import resolve from '@rollup/plugin-node-resolve'
import commonjs from '@rollup/plugin-commonjs'
import babel from 'rollup-plugin-babel'
import { DEFAULT_EXTENSIONS } from '@babel/core'; // .js,.jsx,.es6,.es,.mjs

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  external: [/@babel\/runtime/], //  babelHelpers:'runtime' 使用
  plugins: [
    resolve(),
    commonjs(),
    babel({
      include: 'src/**/*',
      exclude: '**/node_modules/**', 
      babelHelpers:'runtime', // 构建库时使用
      extensions: [...DEFAULT_EXTENSIONS, 'ts'],
    })
  ]
};
```

根目录**babel.config.js**

```js
module.exports = {
  // @babel/env === @babel/preset-env  @babel/react === @babel/preset-react ...
  // 当在presets中使用@babel/env，babel会自动加上preset-
  presets: [
    [
      "@babel/preset-env",
      {
        // esm转换成其他模块语法，cjs、amd、umd等
        // Tree Shaking需要设置为false
        modules: false,
        targets: { browsers: ["> 1%", "last 2 versions", "not ie <= 8"] },
      }
    ],
    "@babel/preset-react",
    "@babel/preset-typescript",
  ],
  plugins: [
    ["@babel/plugin-transform-runtime", {
      corejs: {
        version: 3, // 需安装 @babel/runtime-corejs3
        proposals: true
      }
    }],
    ["@babel/plugin-proposal-decorators", { legacy: true }], // 需要放在@babel/plugin-proposal-class-propertie之前
    ["@babel/plugin-proposal-class-properties", { loose: true }], // 用于解析class语法(react必选)
    // @babel/plugin-proposal-private-methods 和 @babel/plugin-proposal-private-property-in-object内置于preset-env,且他们的loose值必须与@babel/plugin-proposal-class-properties的一致
    ['@babel/plugin-proposal-private-methods', { 'loose': true }],
    ['@babel/plugin-proposal-private-property-in-object', { loose: true }],
  ]
}
```

因为我们可以使用`rollup-plugin-typescript2`编译ts文件，如果使用 babel ，则

```sh
npm i -D 
```

```js
// rollup.config.js
import babel from 'rollup-plugin-babel'


export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  external: [/@babel\/runtime/], //  babelHelpers:'runtime' 使用
  plugins: [
    babel({
      include: 'src/**/*',
      exclude: '**/node_modules/**', 
      babelHelpers:'runtime', // 构建库时使用
      extensions: [...DEFAULT_EXTENSIONS, 'ts','tsx'],
    })
  ]
};
```

```js
module.exports = {
    presets: [
    '@babel/preset-typescript',
    ],
}
```

#### terser

uglify-js只能翻译es5语法。如果要转译es6+语法，请改用terser。

```sh
npm i -D rollup-plugin-terser
```

默认会保留含有"@license", "@copyright", "@preserve" 或 `!`开头的注释。

**roullup.config.js**

```js
import {terser} from 'rollup-plugin-terser';

const isProd = process.env.NODE_ENV === 'production'

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    isProd &&
        terser({
          compress: {
            // https://github.com/terser/terser
            // 默认会去掉debugger
            // drop_console: true, // 去掉console
            pure_funcs:['console.log'] // 如果只想去除console.log，而不想去除console.info
          },
        }),
  ]
};
```

#### 开启本地服务器

```sh
npm i -D rollup-plugin-dev
```

- dirs：静态文件目录
- port：端口号
- proxy：代理

**roullup.config.js**

```js
import dev from 'rollup-plugin-dev'

const isProd = process.env.NODE_ENV === 'production'

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
   !isProd && dev({
           dirs: ['dist'],
      port: 8888,
   })
  ]
};
```

#### 热更新

```sh
npm i -D rollup-plugin-livereload
```

```js
import dev from 'rollup-plugin-dev'
import livereload from 'rollup-plugin-livereload'

const isProd = process.env.NODE_ENV === 'production'

export default {
    plugins:[
    !isProd && dev('dist'),
    !isProd && livereload('dist')

    // --- OR ---

    livereload({
      watch: 'dist',
      verbose: false, // Disable console output

      // other livereload options
      port: 12345,
      delay: 300,
      https: {
          key: fs.readFileSync('keys/agent2-key.pem'),
          cert: fs.readFileSync('keys/agent2-cert.pem')
      }
    })
  ]
}
```

#### 路径别名

```js
// rollup.config.js
import alias from '@rollup/plugin-alias';
import path from 'path';

const srcPath = path.resolve(__dirname,'src');

const options = [
  {
    input: 'src/index.ts',
    output: [
      {
        file: 'dist/index.js',
        format: 'cjs',
      }
    ],
    plugins: [
         alias({
             entries:{
                 '@': srcPath,
             }
         })
    ],
  },
];

export default options;
```

如果采用了TypeScript编写文件，且 tsconfig.json 文件中设置了别名 `compilerOptions.paths:{ "@/*": ["./src/*"],}`，则无需该插件。

#### 分析、进程、清除文件夹

```sh
npm i -D rollup-plugin-clear rollup-plugin-progress rollup-plugin-visualizer
```

```js
import { RollupOptions } from 'rollup';
import typescript from 'rollup-plugin-typescript2';
import babel from '@rollup/plugin-babel';
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';
import { terser } from 'rollup-plugin-terser';
import clear from 'rollup-plugin-clear';
import progress from 'rollup-plugin-progress';
import { visualizer } from 'rollup-plugin-visualizer';

const isProd = process.env.NODE_ENV === 'production';

const options: RollupOptions[] = [
  {
    input: 'src/index.ts',
    output: [
      {
        file: 'dist/index.js',
        format: 'cjs',
      }
    ],
    plugins: [
      clear({
        // required, point out which directories should be clear.
        targets: ['some directory'],
        // optional, whether clear the directores when rollup recompile on --watch mode.
        watch: true, // default: false
      }),
      progress({
        clearLine: false // default: true
      }),
      // put it the last one
      isProd &&
        visualizer({
          filename: './out/report.html',
          open: true,
          gzipSize: true,
        }),
    ],
    external: [/@babel\/runtime/], //  babelHelpers:'runtime' 使用
  },
];

export default options;
```

#### CSS

##### rollup-plugin-postcss

处理css需要用到的插件是`rollup-plugin-postcss`。它支持css文件的加载、css加前缀、css压缩、对scss/less的支持等等。

```js
import postcss from 'rollup-plugin-postcss'
export default {
  input: ...,
  output: ...,
  plugins:[
    postcss()
  ]
}
```

##### css加前缀

借助`autoprefixer`插件来给css3的一些属性加前缀。

```js
import postcss from 'rollup-plugin-postcss'
import autoprefixer from 'autoprefixer'
export default {
  input: ...,
  output: ...,
  plugins:[
    postcss({
      plugins: [  
        autoprefixer()  
      ]
    })
  ]
}
```

使用`autoprefixer`除了以上配置，还需要配置browserslist，有2种方式，一种是建立.browserslistrc文件，另一种是直接在package.json里面配置，我们在package.json中，添加"browserslist"字段。

```json
"browserslist": [
  "defaults",
  "not ie < 8",
  "last 2 versions",
  "> 1%",
  "iOS 7",
  "last 3 iOS versions"
]
```

##### css压缩

cssnano对打包后的css进行压缩。使用方式也很简单，和autoprefixer一样，安装`cssnano`，然后配置。

```js
plugins:[
  postcss({
    plugins: [
      autoprefixer(),
      cssnano()
    ]
  })
]
```

##### 抽离单独的css文件

`rollup-plugin-postcss`可配置是否将css单独分离，默认没有`extract`，css样式生成`style`标签内联到head中，配置了`extract`，就会将css抽离成单独的文件。

```js
postcss({
  plugins: [
    autoprefixer(),
    cssnano()
  ],
  extract: 'css/index.css'  
})
```

##### 支持scss/less

实际项目中我们不太可能直接写css，而是用scss或less等预编译器。`rollup-plugin-postcss`默认集成了对scss、less、stylus的支持。

**With Sass/Stylus/Less**

Install corresponding dependency:

- For `Sass` install `sass`: `yarn add sass --dev`
- For `Stylus` Install `stylus`: `yarn add stylus --dev`
- For `Less` Install `less`: `yarn add less --dev`

That's it, you can now import `.styl` `.scss` `.sass` `.less` files in your library.

**For Sass/Scss Only.**

Similar to how webpack's [sass-loader](https://github.com/webpack-contrib/sass-loader#imports) works, you can prepend the path with `~` to tell this plugin to resolve in `node_modules`:

```css
@import "~bootstrap/dist/css/bootstrap";
```
