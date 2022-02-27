### rollup 和 webpack

**在开发应用时使用 Webpack，开发库时使用 Rollup**

rollup的定位是偏向于JavaScript库的构建，打包的体积也比webpack的小。

Rollup有一项webpack不具有的功能：通过配置output.format开发者可以选择输出资源的模块形式（cjs（CommonJS），esm（ES6 Modules），amd，umd，iife，system等），此特性对于打包JavaScript库特别有用，因为往往一个库需要支持多种不同的模块形式，而通过rollup几个命令就可以把一份源代码打包为多份。

### 使用TypeScript编写配置文件

```
npm i -D @rollup/plugin-typescript
```

项目根目录添加 `tsconfig.json`

**rollup.config.ts**

```ts
export default {
	plugins:[
		typescript()
	]
}
```

```
rollup --config rollup.config.ts --configPlugin typescript
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

此时不应使用 `output.file`，而是使用 `ouput.entryFileNames`。

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



#### Output

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
- `cjs` – CommonJS，适用于 Node 和 Browserify/Webpack
- `esm` – 将软件包保存为 ES 模块文件，在现代浏览器中可以通过 `<script type=module>` 标签引入
- `iife` – 一个自动执行的功能，适合作为`<script>`标签。（如果要为应用程序创建一个捆绑包，您可能想要使用它，因为它会使文件大小变小。）
- `umd` – 通用模块定义，以`amd`，`cjs` 和 `iife` 为一体
- `system` - SystemJS 加载器格式



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

##### output.plugins

Type: `OutputPlugin | (OutputPlugin | void)[]`

与 plugins 选项类似。

不是所有的插件都能在该选项中使用。只有在`bundle.generate()` or `bundle.write()`，即`Rollup's main analysis`完成后使用。

If you are a plugin author, see [output generation hooks](https://rollupjs.org/guide/en/#output-generation-hooks) to find out which hooks can be used.

##### output.globals

Type: `{ [id: string]: string } | ((id: string) => string)`
CLI: `-g`/`--globals <external-id:variableName,another-external-id:anotherVariableName,...>`

`Object` 形式的 `id: name` 键值对，用于`external`文件中`import`的`umd`/`iife`包。例如：

```js
import $ from 'jquery';
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
      jquery: '$'
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

https://github.com/rollup/plugins

| 插件名                                                       | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [alias](https://github.com/rollup/plugins/blob/master/packages/alias) | Define and resolve aliases for bundle dependencies 路径起别名 |
| [auto-install](https://github.com/rollup/plugins/blob/master/packages/auto-install) | Automatically install dependencies that are imported by a bundle |
| [babel](https://github.com/rollup/plugins/blob/master/packages/babel) | Compile your files with Babel                                |
| [beep](https://github.com/rollup/plugins/blob/master/packages/beep) | System beeps on errors and warnings                          |
| [buble](https://github.com/rollup/plugins/blob/master/packages/buble) | Compile ES2015 with buble                                    |
| [commonjs](https://github.com/rollup/plugins/blob/master/packages/commonjs) | 将 CommonJS 转换成 ESM                                       |
| [clear](https://github.com/DongShelton/rollup-plugin-clear)  | Clear an output directory before a build.                    |
| [copy](https://github.com/meuter/rollup-plugin-copy)         | Copy files during a build.                                   |
| [data-uri](https://github.com/rollup/plugins/blob/master/packages/data-uri) | Import modules from Data URIs                                |
| [dsv](https://github.com/rollup/plugins/blob/master/packages/dsv) | Convert .csv and .tsv files into JavaScript modules with d3-dsv |
| [dynamic-import-vars](https://github.com/rollup/plugins/blob/master/packages/dynamic-import-vars) | Resolving dynamic imports that contain variables.            |
| [eslint](https://github.com/rollup/plugins/blob/master/packages/eslint) | Verify entry point and all imported files with ESLint        |
| [graphql](https://github.com/rollup/plugins/blob/master/packages/graphql) | Convert .gql/.graphql files to ES6 modules                   |
| [html](https://github.com/rollup/plugins/blob/master/packages/html) | Create HTML files to serve Rollup bundles                    |
| [image](https://github.com/rollup/plugins/blob/master/packages/image) | Import JPG, PNG, GIF, SVG, and WebP files                    |
| [inject](https://github.com/rollup/plugins/blob/master/packages/inject) | Scan modules for global variables and injects `import` statements where necessary |
| [json](https://github.com/rollup/plugins/blob/master/packages/json) | Convert .json files to ES6 modules                           |
| [legacy](https://github.com/rollup/plugins/blob/master/packages/legacy) | Add `export` declarations to legacy non-module scripts       |
| [multi-entry](https://github.com/rollup/plugins/blob/master/packages/multi-entry) | Use multiple entry points for a bundle                       |
| [node-resolve](https://github.com/rollup/plugins/blob/master/packages/node-resolve) | 从node_modules中查找并打包第三方库                           |
| [replace](https://github.com/rollup/plugins/blob/master/packages/replace) | Replace strings in files while bundling  替换字符串，与webpack.DefinePlugin相同 |
| [run](https://github.com/rollup/plugins/blob/master/packages/run) | Run your bundles in Node once they're built                  |
| [strip](https://github.com/rollup/plugins/blob/master/packages/strip) | Remove debugger statements and functions like assert.equal and console.log from your code |
| [sucrase](https://github.com/rollup/plugins/blob/master/packages/sucrase) | Compile TypeScript, Flow, JSX, etc with Sucrase              |
| [terser](https://github.com/TrySound/rollup-plugin-terser)   | Minify a bundle using Terser                                 |
| [typescript](https://github.com/rollup/plugins/blob/master/packages/typescript) | Integration between Rollup and Typescript                    |
| [url](https://github.com/rollup/plugins/blob/master/packages/url) | Import files as data-URIs or ES Modules                      |
| [virtual](https://github.com/rollup/plugins/blob/master/packages/virtual) | Load virtual modules from memory                             |
| [wasm](https://github.com/rollup/plugins/blob/master/packages/wasm) | Import WebAssembly code with Rollup                          |
| [yaml](https://github.com/rollup/plugins/blob/master/packages/yaml) | Convert YAML files to ES6 modules                            |
| [progress](https://github.com/jkuri/rollup-plugin-progress)  | Show build progress in the console.                          |

### 常用配置

#### TypeScript

```sh
npm install -D rollup typescript tslib @types/node ts-node @rollup/plugin-typescript cross-env
```

**tsconfig.json**

```json
{
  "compilerOptions": {
    "module": "esnext",
    "target": "es5",
    "allowSyntheticDefaultImports": true /* 用来指定允许从没有默认导出的模块中默认导入 */,
    "jsx": "react",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ] /* 编译过程中需要引入的库文件的列表 */,
    "strict": true,
    "moduleResolution": "node",
    "experimentalDecorators": true /* 用于指定是否启用实验性的装饰器特性 */,
    "downlevelIteration": true /* 当target为'ES5' or 'ES3'时，为'for-of', spread, and destructuring'中的迭代器提供完全支持 */,
    "allowJs": true,
    "esModuleInterop": true /* 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性，esModuleInterop选项的作用是支持使用import d from 'cjs'的方式引入commonjs包 */,
    "forceConsistentCasingInFileNames": true /* 	禁止对同一个文件的不一致的引用。 */,
    "noEmit": true /* 不生成编译文件(js) */,
    "noFallthroughCasesInSwitch": true /* 用于检查switch中是否有case没有使用break跳出switch，默认为false */,
    "noImplicitAny": false /* noImplicitAny的值为false时，如果我们没有为一些值设置明确的类型，编译器会默认认为这个值为any，如果noImplicitAny的值为true的话。则没有明确的类型会报错。默认值为false */,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    // 在dist目录生成.d.ts文件
    "outDir": "dist",
    "declaration": true,
    "declarationDir": "."
  },
  "include": ["src", "./roullup.config.ts"],
  "exclude": ["node_modules", "build", "dist"]
}

```

**rollup.config.ts**

```ts
import { RollupOptions } from 'rollup'
import typescript from '@rollup/plugin-typescript'

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
  plugins: [typescript()],
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
npm i -D @babel/core @babel/preset-env core-js@3 @babel/plugin-transform-runtime @babel/runtime-corejs3 @rollup/plugin-babel
```

**rollup.config.js**

**当使用 @rollup/rollup-plugin-commonjs 时，commonjs() 必须放在 babel() 前面**

`babelHelpers`

Type: `'bundled' | 'runtime' | 'inline' | 'external'`
Default: `'bundled'`

**'runtime'** - 如果你要用rollup构建一个js包的时候，使用该配置，该配置要结合@babel/plugin-transform-runtime插件使用，使用@babel/plugin-transform-runtime也要安装@babel/runtime 或 @babel/runtime-corejs2/3 插件。同时设置 `external: [/@babel\/runtime/]`。

**'bundled'** - 如果用rollup构建一个项目应该用此参数。如果希望输出的文件包含helpers函数（最多复制一份），应该用此参数。

**'external'**  要结合@babel/plugin-external-helpers插件使用，它会把helpers 收集到一个全局的共享模块。

**'inline'** 官网不推荐使用，会导致很多重复性代码

```js
import resolve from '@rollup/plugin-node-resolve'
import commonjs from '@rollup/plugin-commonjs'
import babel from 'rollup-plugin-babel'

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
    })
  ]
};
```

**src/babel.config.js**

我们将 `babel.config.js` 文件放在 `src` 中（File-relative configuration），而不是根目录下（Project-wide configuration）。 这允许我们对于不同的任务有不同的 babel 配置。

https://babeljs.io/docs/en/config-files#project-wide-configuration

File-relative configurations are also [merged](https://babeljs.io/docs/en/options#merging) over top of project-wide config values, making them potentially useful for specific overrides, though that can also be accomplished through ["overrides"](https://babeljs.io/docs/en/options#overrides).

```js
module.exports = {
  // 由于@babel/polyfill在7.4.0中被弃用，我们建议直接添加core js并通过corejs选项设置版本
	presets: [
		[
			"@babel/preset-env",
			{
        // esm转换成其他模块语法，cjs、amd、umd等
        // Tree Shaking需要设置为false
				modules: false, 
				targets: { browsers: ["> 1%", "last 2 versions", "not ie <= 8"] },
        // when using useBuiltIns: "usage", set the proposals option to true. This will enable polyfilling of every proposal supported by core-js@xxx
        // 按需加载，将 useBuiltIns 改为 "usage"，babel 就可以按需加载 polyfill，并且不需要手动引入 @babel/polyfill，不过@babel/polyfill已被废弃，请使用安装core-js，并使用corejs选项
				useBuiltIns: "usage", 
				corejs: { 
          version: 3, // 需安装 core-js3.x的版本
          proposals: true, // 支持js提案语法
        }
			}
		],
	],
	plugins: [
		["@babel/plugin-transform-runtime", { 
      corejs: { 
      	version: 3, // 需安装 @babel/runtime-corejs3
        proposals: true 
      }
    }], // 用于babel的编译(必须)，将重复的辅助函数自动替换，节省大量体积
	]
}
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

#### terser

uglify-js只能翻译es5语法。如果要转译es6+语法，请改用terser。

```sh
npm i -D rollup-plugin-terser
```

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
   isProd && terser()
  ]
};
```

