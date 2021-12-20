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

https://docs.npmjs.com/cli/v7/configuring-npm/package-json  

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