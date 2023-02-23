---
title: 正则表达式
tag: 工具
---
## 前言

正则表达式可以说是目前用过的知识工具里面性价比最高的，该工具的功能只有一个: **文本模式匹配**。但是由于网页源代码、后端代码、小说和工作文档全部采用的实际是文本方式，所以正则表达式在这些领域均存在用武之地。

## 用法

核心用法就是写出符合目标文本类的**匹配特征**，因此从以下4个方面来描述特征。

### 字符类

用于描述字符。

|特征字符|特征功能描述|
|---------|-----------|
|\[xyz\]  |xyz中任一字符|
|\[x-z\]  |x-z中任一字符|
|\[^xyz\] |非xyz中的字符|
|.        |任一字符(不包括换行符，取决于flag设置)|
|\d       |数字，等价于\[0-9\]|
|\D       |非数字|
|\w       |拉丁字母表内字符，等价于\[A-Za-z0-9_\](注意包括下划线)|
|\W       |非拉丁字母表内字符|
|\s       |空白字符，等效于\[\f\n\r\t\v\u0020\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff\]|
|\S       |非空白字符|
|\t       |制表符   |
|\r       |回车符   |
|\n       |换行符   |
|\v       |垂直制表符|
|\f       |进纸符   |
|[\b]     |退格符   |
|\0       |NUL字符  |
|\cX      |控制字符表示(不常用)|
|\xhh     |指明字符其数为hh|
|\uhhhh or \u{hhhh} or \u{hhhhh}| Unicode字符|
|\        |表示后面的字符应特殊对待，例如用`\\`来匹配文本中的`\`|
|x\|y     |x或者y|

### 断言类

|断言字符|断言功能描述|
|---------|-----------|
|^         |匹配字符串的开始|
|$         |匹配字符串的结束|
|\b        |匹配单词边界|
|\B        |匹配非单词边界|
|x(?=y)    |前向断言，表示x后面紧跟着y(这里的**前向**表示字符处理的前向，也就是下一个，英文为`Lookahead `)|
|x(?!y)    |前向否定断言，表示x后面不跟着y|
|(?<=y)x   |后向断言，表示x前面有y(这里的**后向**表示字符处理的后向，也就是上一个，英文为`Lookbehind  `)|
|(?<!y)x   |后向否定断言，表示x前面没有y|


### 分组引用类

|分组引用字符|分组引用功能描述|
|---------|-----------|
|(x)      |分组，将x作为一个整体|
|(?:x)    |非捕获分组，将x作为一个整体，但不捕获|
|(?\<name\>x)|命名分组，将x作为一个整体，命名为name|
|\n       |引用第n个分组，n从1开始|
|\k\<name\> |引用名为name的分组|

### 量词类

|量词字符|量词功能描述|
|---------|-----------|
|*        |匹配前面的子表达式零次或多次|
|+        |匹配前面的子表达式一次或多次|
|?        |匹配前面的子表达式零次或一次|
|{n}      |匹配前面的子表达式n次|
|{n,}     |匹配前面的子表达式至少n次|
|{n,m}    |匹配前面的子表达式至少n次，至多m次|
|*?       |匹配前面的子表达式零次或多次，非贪婪模式|


## 注意事项

1. (?:x)和(?=x)的相同点和区别

    相同点：都是非捕获分组，括号内的x都不会作为一个单独的分组出现在结果里；区别：正则的返回结果一般会包括整体结果和分组结果两部分。(?:x)是非捕获分组，会出现整体结果里，而(?=x)是断言，不会出现整体结果里。另外，(x)是捕获分组，会出现整体结果和分组结果里。

    举个例子：
    
    ```python
    import re
    s = "abcab"
    rst = re.search(r"a(?:b)", s) # <re.Match object; span=(0, 2), match='ab'>
    assert len(rst.groups()) == 0
    rst = re.search(r"a(?=b)", s) # <re.Match object; span=(0, 1), match='a'>
    assert len(rst.groups()) == 0
    rst = re.search(r"a(b)", s) # <re.Match object; span=(0, 2), match='ab'>
    assert len(rst.groups()) == 1
    ```
    
2. 前向和后向

    顺着字符处理方向为前向，反之为后向

3. 不同语言对于flag的设置

    一般来说，flag的设置是为了控制匹配的行为，比如是否区分大小写、是否匹配换行符等。不同语言对于flag的设置不同。
    
    比如**python**中的re模块，flag的设置是通过`re.I`、`re.M`等来设置的。
    
    在**javascript**中，flag的设置是通过`/i`、`/m`等来设置的。这里需要注意的是，python中的`re.I`和javascript中的`/i`是等价的，都表示忽略大小写。

    在**Rust**中，flag的设置是通过在正则表达式中加入`(?i)`等来进行设置的。

## 参考

[MDN Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Backreferences)