#### 发包

[发包步骤](https://juejin.cn/post/7052307032971411463)

[创建现代npm包的最佳实践](https://mp.weixin.qq.com/s/YuxSv6TuWylxVNydny4mhg)



##### NPM发包文件黑/白名单

当执行`npm publish`命令，默认包含的文件（不区分大小写）有

```go
package.json
README (and its variants)
CHANGELOG (and its variants)
LICENSE / LICENCE
package.json属性main指向的文件
```

默认忽略的有

```
.git
CVS
.svn
.hg
.lock-wscript
.wafpickle-N
.*.swp
.DS_Store
._*
npm-debug.log
.npmrc
node_modules
config.gypi
*.orig
package-lock.json (use shrinkwrap instead)
```

设置发布文件的黑名单，通过`.gitignore`或`.npmignore`这两个文件来设置忽略的文件或文件夹。

设置发布文件的黑名单，通过`.gitignore`或`.npmignore`这两个文件来设置忽略的文件或文件夹。

如果你在项目中增加了 .npmignore，那么其会完全替代掉 .gitignore 的作用。
想设置发布文件的白名单，设置`package.json`中的`files`属性。 例如

```bash
files:["package.json","src"]
```

这里的优先级是`files>.npmignore>.gitignore`

#### npm package.json 详解

https://juejin.cn/post/6987179395714646024#heading-5

https://docs.npmjs.com/cli/v7/configuring-npm/package-json  

##### Node.js `package.json` field definitions

https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions

##### package.json 非官方字段集合

https://segmentfault.com/a/1190000016365409

##### bin字段

`bin`项用来指定每个内部命令对应的可执行文件的位置。如果你编写的是一个node工具的时候一定会用到`bin`字段。

当我们编写一个`cli`工具的时候，需要指定工具的运行命令，比如常用的`webpack`模块，他的运行命令就是`webpack`。

```json
"bin": {
  "webpack": "bin/index.js",
}
```

当我们执行`webpack`命令的时候就会执行`bin/index.js`文件中的代码。

在模块以依赖的方式被安装，package.json如果存在`bin`选项，会在`node_modules/.bin/`生成对应的文件， `Npm`会寻找这个文件，在node_modules/.bin/目录下建立符号链接。由于node_modules/.bin/目录会在运行时加入系统的`PATH`变量，因此在运行npm时，就可以不带路径，直接通过命令来调用这些脚本。执行结束后，再将 `PATH` 变量恢复原样。

所有node_modules/.bin/目录下的命令，都可以用npm run [命令]的格式运行。在命令行下，键入npm run，然后按tab键，就会显示所有可以使用的命令。

##### main字段

`main`字段指定了加载的入口文件，`require`导入的时候就会加载这个文件。这个字段的默认值是模块根目录下面的`index.js`。

##### module（提案）

指定ESM规范的入口文件，可被webpack、rollup打包器识别。

- 如果打包器已经支持 `module` 字段则会优先使用 `ES6` 模块规范的版本，这样可以启用 `Tree Shaking` 机制。
- 如果打包器还不识别 `module` 字段则会使用我们已经编译成 `CommonJS` 规范的版本，也不会阻碍打包流程。

使得 webpack 、rollup 本身就可以解析ESM模块语法，如果用户只用到 library 的某些部分，则允许通过 [tree shaking](https://webpack.docschina.org/guides/tree-shaking/) 打包更轻量的包。

[This article on Rollup 1.0](https://levelup.gitconnected.com/code-splitting-for-libraries-bundling-for-npm-with-rollup-1-0-2522c7437697#9f6f) says it another way:

> The `main` field makes sure that Node users using `require` will be served the UMD version. 
> 
> The `module` field is not an official npm feature but a common convention among bundlers to designate how to import an ESM version of our library.



##### exports

exports 字段 (https://webpack.js.org/guides/package-exports/)，用于webpack打包

exports 字段声明了一个对应关系，用 import "package" 和 import "package/sub/path" 会返回不同的模块。

这替换了默认返回 main 字段文件的行为。

当指定了 exports 字段时，只有声明了那些模块是可用的，其他的模块会抛出 ModuleNotFound Error。

```js
// package.json
{
  name: 'midash',
  main: './index.js',
  exports: {
    '.': './dist/index.js',
    'get': './dist/get.js'
  }
}

// 正常工作
import get from 'midash/get'

// 无法正常工作，无法引入
import get from 'midash/dist/get'
```

```json
{
  "exports": {
    ".": "./main.js",
    "./sub/path": "./secondary.js",
    "./prefix/": "./directory/",
    "./prefix/deep/": "./other-directory/",
    "./other-prefix/*": "./yet-another/*/*.js"
  }
}
```

根据模块的引用语法，来引用不同的文件

```json
"exports": {
    ".": {
      "import": "./lib/esm/index.mjs",
      "require": "./command.js"
    },
    "./package.json": "./package.json"
  }
```

`exports` 不仅可根据模块化方案不同选择不同的入口文件，还可以根据环境变量(`NODE_ENV`)、运行环境(`nodejs`/`browser`/`electron`) 导入不同的入口文件

```json
{
  "type": "module",
  "exports": {
    "electron": {
      "node": {
        "development": {
          "module": "./index-electron-node-with-devtools.js",
          "import": "./wrapper-electron-node-with-devtools.js",
          "require": "./index-electron-node-with-devtools.cjs"
        },
        "production": {
          "module": "./index-electron-node-optimized.js",
          "import": "./wrapper-electron-node-optimized.js",
          "require": "./index-electron-node-optimized.cjs"
        },
        "default": "./wrapper-electron-node-process-env.cjs"
      },
      "development": "./index-electron-with-devtools.js",
      "production": "./index-electron-optimized.js",
      "default": "./index-electron-optimized.js"
    },
    "node": {
      "development": {
        "module": "./index-node-with-devtools.js",
        "import": "./wrapper-node-with-devtools.js",
        "require": "./index-node-with-devtools.cjs"
      },
      "production": {
        "module": "./index-node-optimized.js",
        "import": "./wrapper-node-optimized.js",
        "require": "./index-node-optimized.cjs"
      },
      "default": "./wrapper-node-process-env.cjs"
    },
    "development": "./index-with-devtools.js",
    "production": "./index-optimized.js",
    "default": "./index-optimized.js"
  }
}
```



##### typings

TypeScript 的入口文件

```json
{
  "typings": "dist/index.d.ts",
}
```

##### dependencies 和 devDependencies

- dependencies：您的应用程序在生产中所需的包。
- devDependencies：只需要本地开发和测试的包

**区别主要有如下两点：**

- 如果使用 --production 参数，只安装 dependencies 字段的模块。

```sh
$ npm install --production
或者
$ NODE_ENV=production npm install
```

对于自己要发布上线的项目，eslint、prettier以及ts的type等等这些发布完全用不到的可以放到`devDependencies`下，然后上线打包部署的时候可以用`npm install --production`可以减少一些依赖安装时间吧，**前提一定是不参与打包的**

- 如果我们对外提供的是一个依赖包，在别人引用我们的包的时候devDependencies中的依赖不会被 npm 下

**对于应用开发而言**

是否将模块打包和dependencies、devDependencies没有任何关系，只要引入且使用了模块，就会被模块打包器识别并打包。

**对于库 (Package) 开发而言，是有严格区分的**

- dependencies: 在生产环境中使用
- devDependencies: 在开发环境中使用，如 webpack/babel/eslint 等

当在项目中安装一个依赖的 Package 时，该依赖的 `dependencies` 也会安装到项目中，即被下载到 `node_modules` 目录中。但是 `devDependencies` 不会。

##### peerDependencies

目的是提示宿主环境去安装满足库peerDependencies所指定依赖的包，然后在import或者require所依赖的包的时候，永远都是引用宿主环境统一安装的npm包，最终解决库与所依赖包不一致的问题。

>例如 `ant-design` 的 peerDependencies : "react": ">=16.9.0"
>
>意思是 `ant-design` 依赖对应版本的react，同时要求宿主环境安装该范围版本的react，避免 `ant-design` 引用 react 出错。

比方其中有一个依赖包PackageA，该包的package.json文件指定了对PackageB的依赖：

```json
{
    "dependencies": {
        "PackageB": "1.0.0"
    }
}
```

如果我们在我们的MyProject项目中执行npm install PackageA, 我们会发现我们项目的目录结构会是如下形式：

```ruby
MyProject
|- node_modules
   |- PackageA
      |- node_modules
         |- PackageB
```

那么在我们的项目中，我们能通过下面语句引入"PackageA"：

```jsx
var packageA = require('PackageA')
```

但是，如果你想在项目中直接引用PackageB:

```jsx
var packageA = require('PackageA')
var packageB = require('PackageB')
```

这是不行的，即使PackageB被安装过；因为Node只会在“MyProject/node_modules”目录下查找PackageB，它不会在进入PackageA模块下的node_modules下查找。

所以，为了解决这个问题，在MyProject项目package.json中我们必须直接声明对PackageB的依赖并安装。

**peerDependencies的引入**

例如上面PackageA的package.json文件如果是下面这样：

版本号通常是范围 `~`、`^`

```json
{
    "peerDependencies": {
        "PackageB": "1.0.0"
    }
}
```

那么，它会告诉npm：如果某个package把我列为依赖的话，那么那个package也必需应该有对PackageB的依赖。

也就是说，如果你npm install PackageA，你将会得到下面的如下的目录结构：

```ruby
MyProject
|- node_modules
   |- PackageA
   |- PackageB
```

In npm versions 3 through 6, `peerDependencies` were not automatically installed, and would raise a warning if an invalid version of the peer dependency was found in the tree. As of npm v7, peerDependencies *are* installed by default.

##### files

当你发布package时，具体那些文件会发布上去

```json
"files": [
  "src",
  "dist/*.js",
  "types/*.d.ts"
],
```

**sideEffects**

用于webpack，声明该模块是否包含 sideEffects（副作用），从而可以为 tree-shaking 提供更大的优化空间

##### unpkg

```json
{
  "unpkg": "dist/jquery.js"
}
```

正常情况下，访问 `jquery` 的发布文件通过 `https://unpkg.com/jquery@3.3.1/dist/jquery.js`，当你使用省略的 url `https://unpkg.com/jquery` 时，便会按照如下的方式获取文件：

```asciidoc
# [latestVersion] 指最新版本号，pkg 指 package.json

# 定义了 unpkg 属性时
https://unpkg.com/jquery@[latestVersion]/[pkg.unpkg]

# 未定义 unpkg 属性时，将回退到 main 字段
https://unpkg.com/jquery@[latestVersion]/[pkg.main] 
```



#### npm link

**用法**

cd到模块目录，npm link，进行全局link

cd到项目目录，npm link 模块名(模块package.json中的name)

**解除link**

解除项目和模块link，项目目录下，npm unlink 模块名

解除模块全局link，模块目录下，npm unlink 模块名

**详解**

- **建立链接**

假设项目名称为`project1`，和一个公用组件模块`common`，现需要在项目中使用`common`，且`common`是作为npm打包成项目依赖。

首先第一步，使用`npm link`将`common`模块创建成本地依赖包。在`common`目录下输入命令：

```sh
npm link
```

然后进入到`project1`项目目录里，和本地`common`模块建立链接。命令中"common"是`common`模块中package.json的**name**属性值，而不是目录名称。

```sh
npm link common
```

现在在project1中的node_models里就会添加一个`common`模块的软连接。就说明项目链接模块成功了。
之后修改`common`里的内容就会实时更新，而不用打包发布再安装依赖。

- **解除链接**

解除项目的依赖直接在项目目录里输入命令：

```sh
npm unlink common
```

这样项目里就解除了`common`模块的软连接，然后可以在输入`npm install common`安装你发布更新好的`common`模块包。

要解除本地`common`包，在`common`目录中输入命令：

```sh
npm unlink common
```

这样本地的`common`包模块就解除了，其他项目的软连接也失效了。

#### npm version 版本

[规范升级npm包](https://mp.weixin.qq.com/s/ujWW6vTtwTcuiM8rj2vNTQ)

方式一：修改版本号：使用 `npm version <update_type>` 进行修改，`update_type` 有三个参数

- `major`: 当你发了一个含有 Breaking Change 的 API
- `minor`: 当你新增了一个向后兼容的功能时
- `patch`: 当你修复了一个向后兼容的 Bug 时

> 比如我想来个1.0.1版本，注意，是最后一位修改了增1，那么命令：npm version patch
> 比如我想来个1.1.0版本，注意，是第二位修改了增1，那么命令： npm version minor
> 比如我想来个2.0.0版本，注意，是第一位修改了增1，那么命令： npm version major

方式二：修改 package.json 的 version。

#### 版本号~和^和*的区别

- `~` 会匹配最近的 patch 版本依赖包，版本号范围是 `>=1.2.3 <1.3.0`
- `^` 会匹配最新的 minor 版本依赖包，版本号范围是 `>=1.2.3 <2.0.0`
- `* ` 这意味着安装最新版本的依赖包

推荐使用~，只会修复版本的bug，比较稳定。

当我们 `npm i` 时，默认的版本号是 `^`，可最大限度地在向后兼容与新特性之间做取舍。

##### 升级版本号

使用 `npm outdated` 虽能发现需要升级版本号的 package，但仍然需要手动在 package.json 更改版本号进行升级。

此时推荐一个功能更强大的工具 `npm-check-updates`，比 `npm outdated` 强大百倍。

`npm-check-updates -u`，可自动将 package.json 中待更新版本号进行重写。

升级 [minor] 小版本号，有可能引起 `Break Change`，可仅仅升级到最新的 patch 版本。

```bash
$ npx npm-check-updates --target patch
```



**npm i 某个 package 时会修改 `package-lock.json` 中的版本号吗**

当 `package-lock.json` 中该 package 锁死的版本号符合 `package.json` 中的版本号范围时，将以 `package-lock.json` 锁死版本号为主。

当 `package-lock.json` 中该 package 锁死的版本号不符合 `package.json` 中的版本号范围时，将会安装该 package 符合 `package.json` 版本号范围的最新版本号，并重写 `package-lock.json`。



#### npm 工具

##### nrm

nrm 是一个 npm 源管理器,允许你快速地在 npm源间切换

```sh
nrm ls
nrm use taobao
```

##### nodemon

nodemon实时监听文件的变更，自动重启node服务

##### pm2

PM2是一个带有负载均衡功能的 Node 应用进程管理器。

##### serve

```sh
npm install serve -g
serve folder_name
# or
npx serve folder_name
yarn create serve folder_name
```

默认获取 folder_name/index.html

##### dir-parser

[GitHub - CN-Tower/dir-parser: Parse a directory and generate it&#39;s structure tree. 文件夹分析工具，解析文件夹并生成内部文件信息及其文件树。可以使用命令行，也可以在js代码中使用。](https://github.com/CN-Tower/dir-parser)

#### npm scripts hooks

| 命令 | 说明                                                       |
| ---- | ---------------------------------------------------------- |
| `&&` | 顺序执行多条命令，当碰到执行出错的命令后将不执行后面的命令 |
| `&`  | 并行执行多条命令                                           |
| `||` | 顺序执行多条命令，当碰到执行正确的命令将不执行后面的命令   |
| `|`  | 管道符                                                     |

http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html

https://docs.npmjs.com/cli/v8/using-npm/scripts#npm-rebuild

##### npm install

These also run when you run `npm install -g <pkg-name>`

- `preinstall`
- `install`
- `postinstall`
- `prepublish`
- `preprepare`
- `prepare`
- `postprepare`

##### npm publish

- `prepublishOnly`
- `prepack`
- `prepare`
- `postpack`
- `publish`
- `postpublish`

##### npm ci

- `preinstall`
- `install`
- `postinstall`
- `prepublish`
- `preprepare`
- `prepare`
- `postprepare`

##### npm run

- `pre<user-defined>`
- `<user-defined>`
- `post<user-defined>`

npm 脚本有`pre`和`post`两个钩子。举例来说，`build`脚本命令的钩子就是`prebuild`和`postbuild`

```
"prebuild": "echo I run before the build script",
"build": "cross-env NODE_ENV=production webpack",
"postbuild": "echo I run after the build script"
```

用户执行`npm run build`的时候，会自动按照下面的顺序执行

```shell
npm run prebuild && npm run build && npm run postbuild
```





#### 发布scope包

一  登录npm 个人中心 添加或新建组织

二  更改包名称

package.json：

> name: "@aaa/bbb"

三 发包

然后进行登录，输入你注册的账号密码邮箱：

```bash
npm login
```

还可以用下面命令退出当前账号

```bash
npm logout
```

如果不知道当前登录的账号可以用who命令查看身份：

```bash
npm who am i
```

登录成功就可以将我们的包推送到服务器上去了，执行下面命令，会看到一堆的npm notice：

发布public库免费且无限制，发布private库需要收费7$/mon，因此我们公共库需要加上 `--access public` 

```bash
npm publish --access public
```

如果某版本的包有问题，我们还可以将其撤回

```bash
npm unpublish [pkg]@[version]
```



#### 分析

[NPM.DEVTOOL.TECH](https://npm.devtool.tech/)



#### Command

##### --save  --no-save

> Now install Express in the myapp directory and save it in the dependencies list. For example:

```ruby
$ npm install express --save
```

> To install Express temporarily and not add it to the dependencies list:

```ruby
$ npm install express --no-save
```



#### 常见问题

忽略某个包的脚本执行

```
npm install foo --ignore-scripts
```

