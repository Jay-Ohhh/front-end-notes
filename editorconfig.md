vscode下载**EditorConfig for VS Code**插件

通配符

```
*                匹配除/之外的任意字符串
**               匹配任意字符串
?                匹配任意单个字符
[name]           匹配name中的任意一个单一字符
[!name]          匹配不存在name中的任意一个单一字符
{s1,s2,s3}       匹配给定的字符串中的任意一个(用逗号分隔) 
{num1..num2}   　匹配num1到num2之间的任意一个整数, 这里的num1和num2可以为正整数也可以为负整数
```

配置选项

```
root = true                         # 根目录的配置文件，编辑器会由当前目录向上查找，如果找到 `roor = true` 的文件，则不再查找

[*]                                 # 匹配所有的文件
indent_style = space                # 空格缩进
indent_size = 4                     # 缩进空格为4个
end_of_line = lf                    # 文件换行符是 linux 的 `\n`
charset = utf-8                     # 文件编码是 utf-8
trim_trailing_whitespace = true     # 不保留行末的空格
insert_final_newline = true         # 文件末尾添加一个空行
curly_bracket_next_line = false     # 大括号不另起一行
spaces_around_operators = true      # 运算符两遍都有空格
indent_brace_style = 1tbs           # 条件语句格式是 1tbs
spaces_around_brackets              # 表示括号和括号之间应有空格：无空格，仅在括号内，仅在括号外或在括号的两侧 （none，inside，outside，both）
max_line_length                     # 在指定的字符数后强制换行。off关闭此功能
[*.js]                              # 对所有的 js 文件生效
quote_type = single                 # 字符串使用单引号

[*.{html,less,css,json}]            # 对所有 html, less, css, json 文件生效
quote_type = double                 # 字符串使用双引号

[package.json]                      # 对 package.json 生效
indent_size = 2                     # 使用2个空格缩进
```

常用配置

```
# http://editorconfig.org
# root: 表明是最顶层的配置文件，发现设为true时，才会停止查找.editorconfig文件。
root = true

[*]
# 文件编码
charset = utf-8
# 使用单引号
quote_type = single
# 换行符格式
end_of_line = lf
# 缩进数量
indent_size = 2
# 缩进类型
indent_style = tab
# 行最大长度
max_line_length = 80
# 删除末尾空格
trim_trailing_whitespace = true
# 结尾插入新行
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false

[*.{html,css,less,scss,json}]       # 对所有 html,css,less,scss,json 文件生效
quote_type = double                 # 字符串使用双引号
```

