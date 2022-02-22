### rollup 和 webpack

**在开发应用时使用 Webpack，开发库时使用 Rollup**

rollup的定位是偏向于JavaScript库的构建，打包的体积也比webpack的小。

Rollup有一项webpack不具有的功能：通过配置output.format开发者可以选择输出资源的模块形式（cjs（CommonJS），esm（ES6 Modules），amd，umd，iife，system等），此特性对于打包JavaScript库特别有用，因为往往一个库需要支持多种不同的模块形式，而通过rollup几个命令就可以把一份源代码打包为多份。

### 配置选项

```js
// rollup.config.js
export default {
  // 核心选项
  input,     // 必须
  external,
  plugins,

  // 额外选项
  onwarn,
  watch,

  // danger zone
  acorn,
  context,
  moduleContext,
  legacy

  output: {  // 必须 (如果要输出多个，可以是一个数组)
    // 核心选项
    file,    // 必须
    format,  // 必须
    name,
    globals,

    // 额外选项
    paths,
    banner,
    footer,
    intro,
    outro,
    sourcemap,
    sourcemapFile,
    interop,

    // 高危选项
    exports,
    amd,
    indent
    strict
  },
};
```

#### external、globals、peerDependencies



### 命令行参数

```
-i, --input <filename>      要打包的文件（必须）
-o, --file <output>         输出的文件 (如果没有这个参数，则直接输出到控制台)
-f, --format <format>       输出的文件类型 (amd, cjs, esm, iife, umd)
-e, --external <ids>        将模块ID的逗号分隔列表排除
-g, --globals <pairs>       以`module ID:Global` 键值对的形式，用逗号分隔开 
                              任何定义在这里模块ID定义添加到外部依赖
-n, --name <name>           生成UMD模块的名字
-h, --help                  输出 help 信息
-m, --sourcemap             生成 sourcemap (`-m inline` for inline map)
--amd.id                    AMD模块的ID，默认是个匿名函数
--amd.define                使用Function来代替`define`
--no-strict                 在生成的包中省略`"use strict";`
--no-conflict               对于UMD模块来说，给全局变量生成一个无冲突的方法
--intro                     在打包好的文件的块的内部(wrapper内部)的最顶部插入一段内容
--outro                     在打包好的文件的块的内部(wrapper内部)的最底部插入一段内容
--banner                    在打包好的文件的块的外部(wrapper外部)的最顶部插入一段内容
--footer                    在打包好的文件的块的外部(wrapper外部)的最底部插入一段内容
--interop                   包含公共的模块（这个选项是默认添加的）

-h,--help                   打印帮助文档
-v,--version                打印已安装的Rollup版本号
-w,--watch                  监听源文件是否有改动，如果有改动，重新打包
--silent                    不要将警告打印到控制台
```

`rollup -c` 默认使用 `rollup.config.js`，若使用其他配置文件

```sh
rollup --config rollup.config.dev.js
rollup --config rollup.config.prod.js
```

### 插件

插件大全

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

#### @rollup/rollup-plugin-babel

**rollup.config.js**

**当使用 @rollup/rollup-plugin-commonjs 时，commonjs() 必须放在 babel() 前面**

```js
import resolve from 'rollup-plugin-node-resolve';
import commonjs from '@rollup/rollup-plugin-commonjs';
import babel from 'rollup-plugin-babel';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    resolve(),
    commonjs(),
    babel({
      exclude: '**/node_modules/**' // 只编译我们的源代码
    })
  ]
};
```

**src/babel.config.js**

我们将 `babel.config.js` 文件放在 `src` 中（File-relative configuration），而不是根目录下（Project-wide configuration）。 这允许我们对于不同的任务有不同的 babel 配置。

https://babeljs.io/docs/en/config-files#project-wide-configuration

File-relative configurations are also [merged](https://babeljs.io/docs/en/options#merging) over top of project-wide config values, making them potentially useful for specific overrides, though that can also be accomplished through ["overrides"](https://babeljs.io/docs/en/options#overrides).

```sh
npm i -D @babel/core @babel/preset-env core-js@3 @babel/plugin-transform-runtime babel/runtime-corejs3
```

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

