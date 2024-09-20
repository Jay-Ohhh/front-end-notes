# 命名规范

## key的命名规范

- 全部大写
- key 单词与单词之间以 ：分开

- 

> 1) 第一段放置数据库名 如 PRO
>
> 2. 第二段把表名转换为key前缀 如, USER
>
> 3. 第三段放置用于区分区key的字段,对应mysql中的主键的列名,如 UID
>
> 4. 第四段放置主键值,如18,16
>
> e.g.  PRO:USER:UID:18
>
> 每一个冒号都会相当于使用一个文件夹进行分组，例如 PRO:USER:UID:18 = PRO/USER/UID/18
>
> 如果使用双冒号，则等于一个无名称的文件夹，例如 PRO::USER:UID:18  = PRO/无名称文件夹/USER/UID/18



# [亿级系统的Redis缓存如何设计](https://mp.weixin.qq.com/s/mc1zzjy5fEbXCxwhJoWA2Q)



# [Redis缓存那点破事 | 绝杀面试官 25 问](https://mp.weixin.qq.com/s/RRVCJS_X60ugA_52h38TBA)