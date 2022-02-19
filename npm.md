#### 发包

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

在模块以依赖的方式被安装，如果存在`bin`选项。在`node_modules/.bin/`生成对应的文件， `Npm`会寻找这个文件，在node_modules/.bin/目录下建立符号链接。由于node_modules/.bin/目录会在运行时加入系统的`PATH`变量，因此在运行npm时，就可以不带路径，直接通过命令来调用这些脚本。执行结束后，再将 `PATH` 变量恢复原样。

所有node_modules/.bin/目录下的命令，都可以用npm run [命令]的格式运行。在命令行下，键入npm run，然后按tab键，就会显示所有可以使用的命令。

#####  main字段

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

##### typings 

TypeScript 的入口文件

```json
{
	"typings": "dist/index.d.ts",
}
```

##### peerDependencies

目的是提示宿主环境去安装满足插件peerDependencies所指定依赖的包，然后在插件import或者require所依赖的包的时候，永远都是引用宿主环境统一安装的npm包，最终解决插件与所依赖包不一致的问题。

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

cd到项目目录，npm link 模块名(package.json中的name)

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

#### npm version 更新版本

方式一：修改版本号：使用 `npm version <update_type>` 进行修改，`update_type` 有三个参数

- patch：这个是补丁的意思，补丁最合适；
- minor：这个是小修小改；
- major：这个是大改咯；

> 比如我想来个1.0.1版本，注意，是最后一位修改了增1，那么命令：npm version patch
> 比如我想来个1.1.0版本，注意，是第二位修改了增1，那么命令： npm version minor
> 比如我想来个2.0.0版本，注意，是第一位修改了增1，那么命令： npm version major

方式二：修改 package.json 的 version。

#### npm 工具

##### nrm

nrm 是一个 npm 源管理器,允许你快速地在 npm源间切换

##### nodemon

nodemon实时监听文件的变更，自动重启node服务

##### pm2

PM2是一个带有负载均衡功能的 Node 应用进程管理器。

##### serve

```sh
npm install serve -g
serve folder_name
```

默认获取 folder_name/index.html

##### tree

```bash
cd 目标文件夹路径
```

然后 tree 一下

```undefined
tree
```

选项

| 选项       | 含义                                                   |
| ---------- | ------------------------------------------------------ |
| -a         | 显示所有文件和目录                                     |
| -d         | 只显示目录名称，不显示文件                             |
| -D         | 列出文件或目录的更改时间                               |
| -L num     | 显示num层目录结构，深度大禹num层的目录和文件将不会显示 |
| - I        | 过滤文件或文件夹                                       |
| - e '文件' | 输出到某文件                                           |

tree -I pattern 用于过滤不想要显示的文件或者文件夹。比如要过滤项目中的node_modules文件夹：

```
tree -I “node_modules”
```

#### npm scripts hooks

http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html
