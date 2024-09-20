#### process.cwd() 、__dirname、require.main

- process.cwd() 是当前执行node命令时候的文件夹绝对路径

> The `process.cwd()` method returns the current working directory of the Node.js process.

- __dirname 当前文件所属目录的绝对路径
- require.main 

> ### Accessing the main module[#](https://nodejs.org/dist/latest-v16.x/docs/api/modules.html#accessing-the-main-module)
>
> When a file is run directly from Node.js, `require.main` is set to its `module`. That means that it is possible to determine whether a file has been run directly by testing `require.main === module`.
>
> For a file `foo.js`, this will be `true` if run via `node foo.js`, but `false` if run by `require('./foo')`.
>
> When the entry point is not a CommonJS module, `require.main` is `undefined`, and the main module is out of reach.

#### `__dirname` 、`__filename`

- `__dirname` 可以用来**动态获取**当前文件所属目录的绝对路径
- `__filename` 可以用来**动态获取**当前文件的绝对路径

#### path.resolve

- `...paths` [](http://url.nodejs.cn/9Tw2bK) 路径或路径片段的序列
- 返回: [](http://url.nodejs.cn/9Tw2bK)

`path.resolve()` 方法将路径或路径片段的序列解析为绝对路径。

给定的路径序列从右到左处理，每个后续的 `path` 会被追加到前面，直到构建绝对路径。 例如，给定路径片段的序列：`/foo`、`/bar`、`baz`，调用 `path.resolve('/foo', '/bar', 'baz')` 将返回 `/bar/baz`，因为 `'baz'` 不是绝对路径，而 `'/bar' + '/' + 'baz'` 是。

如果在处理完所有给定的 `path` 片段之后，**还没有生成绝对路径，则在前面加上当前工作目录**。

零长度的 `path` 片段被忽略。

如果没有传入 `path` 片段，则 `path.resolve()` 将返回当前工作目录的绝对路径。

> 绝对路径形式：/a/b
> 
> 非绝对路径形式：a/b

**对于`/a`**，称为一个完整路径；例如 /a/b/  ,  `/b`是一个完整路径

若path片段以 `./` 开头 或者 没有符号 则以 `路径名称` 拼接前面路径；

若path片段以 `../` 开头，将前面路径最后一个的**完整路径**删除，以 `路径名称` 拼接前面路径；

若path是 `..` ，则将前面拼接的路径最后一个的**完整路径**删除

若path是 `/` ，不会被删掉，而是直接与后面的path构成绝对路径

生成的路径被规范化，并删除尾部斜杠（除非路径解析为根目录）。

> /a/b/ 和 /a/b 最后一个的**路径名称**是 b
> 
> 若拼接路径的 `/` 重复，则取一个 `/`，例如：/a//src  =>  /a/src

```js
// 假设当前工作目录为 /home
const path = require('path');

path.resolve('a', 'src') // return /home/a/src
path.resolve('a', './src') // return /home/a/src
path.resolve('/a', 'src') // return /home/a/src
path.resolve('a','/src') //  /home/src
path.resolve('/a', '../src') // /home/src
path.resolve('/a','..','src') // /home/src
```

#### path.join()

- `...paths` [](http://url.nodejs.cn/9Tw2bK) 路径片段的序列
- 返回: [](http://url.nodejs.cn/9Tw2bK)

`path.join()` 方法使用特定于平台的分隔符作为定界符将所有给定的 `path` 片段连接在一起，然后规范化生成的路径。

零长度的 `path` 片段被忽略。 如果连接的路径字符串是零长度字符串，则将返回 `'.'`，表示当前工作目录。

若字符以 / 开头，则会忽略/，拼接到前面路径；

如果在处理完所有给定的 path 片段之后还未生成绝对路径，**不会在前面加上当前工作目录**。

```js
const path = require('path');
path.join('/a', '/src') // /a/src
path.join('a', '/src') // a/src
path.join('a', '../src') // src
path.join('/a/b', '..', 'src') // /a/src
```

#### 字符串路径

字符串路径被解释为标识绝对或相对文件名的 UTF-8 字符序列。 相对路径将相对于通过调用 `process.cwd()` 确定的当前工作目录进行解析。

在 POSIX 上使用绝对路径的示例：

```js
import { open } from 'fs/promises';

let fd;
try {
  fd = await open('/open/some/file.txt', 'r');
  // 对文件做一些事情
} finally {
  await fd.close();
}
```

在 POSIX 上使用相对路径的示例（相对于 `process.cwd()`）:

```js
import { open } from 'fs/promises';

let fd;
try {
  fd = await open('file.txt', 'r');
  // 对文件做一些事情
} finally {
  await fd.close();
}
```

#### console VS process.stdout

**Difference between process.stdout.write and console.log in Node.js are:**

| | **process.std.out**                                          | **console.log**                                              |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1      | It continuously prints the information as the data being retrieved and doesn’t add a new line. | It prints the information that was obtained at the point of retrieval and adds a new line. |
| 2      | Using **process.stdout** for a variable that shows an object. | Using **console.log** for a variable shows a lot of unreadable characters. |
| 3      | It only takes strings as arguments. Any other data type passed as a parameter will throw a TypeError. | It takes any JavaScript data type.                           |
| 4      | If we don’t put the break line at the end we will get a weird character after our string. | We don’t need the break line here because it was already formatted and also that weird character did disappear |
| 5      | It can be useful for printing patterns as it does not add a new line. | It is used when we want our result to be printed in a new line. |
| 6      | We can not write more than one string. For example: `process.stdout.write("Hello","World");` Output: This will give a type error. | We can write more than one string. For example: `console.log("Hello", "World");` Output: This will print Hello World in the console. |
| 7 | We can not make associations. For example:`process.stdout.write("Hello %s", "All");`Output: This will give a type error. | We can make associations. For example:`console.log("Hello %s", "All");`Output: This will print Hello All in the console. |
