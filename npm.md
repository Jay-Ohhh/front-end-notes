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

##### bin字段

`bin`项用来指定每个内部命令对应的可执行文件的位置。如果你编写的是一个node工具的时候一定会用到`bin`字段。

当我们编写一个`cli`工具的时候，需要指定工具的运行命令，比如常用的`webpack`模块，他的运行命令就是`webpack`。

```json
"bin": {
  "webpack": "bin/index.js",
}
复制代码
```

当我们执行`webpack`命令的时候就会执行`bin/index.js`文件中的代码。

在模块以依赖的方式被安装，如果存在`bin`选项。在`node_modules/.bin/`生成对应的文件， `Npm`会寻找这个文件，在node_modules/.bin/目录下建立符号链接。由于node_modules/.bin/目录会在运行时加入系统的PATH变量，因此在运行npm时，就可以不带路径，直接通过命令来调用这些脚本。

所有node_modules/.bin/目录下的命令，都可以用npm run [命令]的格式运行。在命令行下，键入npm run，然后按tab键，就会显示所有可以使用的命令。

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
