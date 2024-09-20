# ssh/openssh

https://wiki.archlinux.org/title/SSH_keys_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

https://wiki.archlinux.org/title/OpenSSH_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

# 上传文件到服务器

**连接服务器（这一步可忽略）**

```shell
# 以下两种方式都可以
ssh [-p port] username@ip 
# exit/logout 结束连接
```

Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

然后输入密码即可。



**scp命令传输文件**

```shell
# 本地shell
# -r 递归复制整个目录
scp -r [-P 端口号] 本地文件 用户名@服务器ip:目标文件夹 # 上传
scp -r [-P 端口号] 用户名@服务器ip:目标文件夹 本地文件 # 下载
```

然后，输入"yes"，回车。
最后，输入密码，回车。



# 命令行快捷键

| description                      | mac                    |      |
| -------------------------------- | ---------------------- | ---- |
| 往回删除一个单词，光标放在最末尾 | ctrl + w               |      |
| 删除光标以前的字符               | ctrl + u               |      |
| 删除光标以后的字符               | ctrl + k               |      |
| 移动光标至行首                   | ctrl + a / command + ⬅️ |      |
| 移动光标至行尾                   | ctrl + e / command + ➡️ |      |
| 清屏                             | ctrl + l / command + k |      |



# $0 $1 ...

在 Shell 中，$0、$1、$2 等都是特殊变量，用于表示脚本或函数的参数。具体来说：

- $0 表示当前脚本或命令的名称，通常是一个文件名，例如：/usr/local/bin/myscript。
- $1 表示第一个参数，即脚本或函数调用时传入的第一个参数，例如：如果调用脚本时使用了命令 myscript.sh arg1 arg2，则 $1 表示 arg1。
- $2 表示第二个参数，以此类推，$3、$4 表示第三个、第四个参数，依次类推。

```bash
#!/bin/bash

echo "脚本名称：$0"
echo "第一个参数：$1"
echo "第二个参数：$2"
```

当执行这个脚本时，可以传递不同的参数，例如：

```bash
$ ./myscript.sh hello world
```

执行结果为：

```bash
脚本名称：./myscript.sh
第一个参数：hello
第二个参数：world
```



**cd "$(dirname "$0")"**

这是一个在 bash shell 脚本中常用的命令，它的作用是将当前工作目录切换到脚本所在目录。

具体地说，这个命令中的 $0 表示当前脚本的名称，而 dirname "$0" 表示获取当前脚本所在的目录名。这里使用双引号是为了防止脚本名称或目录名中包含空格等特殊字符导致的错误。

最后，使用 $() 将 dirname "$0" 的结果作为参数传递给 cd 命令，以切换到当前脚本所在的目录。
