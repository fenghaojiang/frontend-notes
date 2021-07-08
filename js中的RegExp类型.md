---
title: js中的RegExp类型
date: 2021-07-08
---  

### 5.4 RegExp类型  

使用下面类似Perl的语法，就可以创建一个正则表达式。

```js
var expression = / pattern / flags ;
```

其中的模式(pattern)部分可以是任何简单或复杂的正则表达式，可以包含字符类、限定符、分组、向前查找以及反向引用。每个正则表达式都可带有一个或多个标志(flags)， 用以标明正则表达式的行为。正则表达式的匹配模式支持下列3个标志。

+ g: 表示全局(global)模式，即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止；
+ i: 表示不区分大小写(case-insensitive)模式，即在确定匹配项时忽略模式与字符串的大小写； 
+ m: 表示多行(multiline)模式，即在到达一行文本末尾时还会继续查找下一行是否存在与模式匹配的项  

```js
var pattern1 = /at/g; //匹配字符串中所有的at实例
var pattern2 = /[bc]at/i; //匹配第一个bat或者cat
var pattern3 = /.at/gi //匹配所有以at结尾的3个字符的组合，不区分大小写
```  


---


```js
var pattern1 = /[bc]at/i;
var pattern2 = new RegExp("[bc]at", "i"); //ps: 字符串要转义  
```
在此，pattern1和pattern2是完全等价的正则表达式。  


|字面量模式|等价的字符串|
|:---:|:---:|
|/w\\hello\\123|\\w\\\\hellow\\\\123|  




