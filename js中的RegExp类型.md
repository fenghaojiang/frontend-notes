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


```js
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;

var matches = pattern.exec(text);

alert(matches[0]); //"mom and dad and baby"
alert(matches[1]); //" and dad and baby"
alert(matches[2]); //" and baby"
```  

正则表达式的第二个方法是test(),它接受一个字符串参数。在模式与该参数匹配的情况下返回true;否则，返回false。在只想知道目标字符串与某个模式是否匹配，但不需要知道其文本内容的情况下，使用这个方法非常方便。因此，test()方法经常被用在if语句中  


```js
var text = "000-00-0000";
var pattern = /\d{3}-\d{2}-\d{4}/;
if (pattern.test(text)) {
    alert("the pattern was matched.");
}
```

### 5.5.2 函数声明与函数表达式  

本机