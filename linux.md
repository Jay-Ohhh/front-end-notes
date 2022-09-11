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







