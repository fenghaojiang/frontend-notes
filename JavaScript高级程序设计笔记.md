---
title: JavaScript高级程序设计笔记
date: 2021-06-28
---  


## 3.7.1 理解函数

### arguments用法
```js
function doAdd(num1, num2) {
    arguments[1] = 10;
    alert(arguments[0] + num2);
}
```

每次执行这个doAdd()函数都会重写第二个参数，将第二个参数的值改为10。因为arguments对象中的值会自动反映到对应的命名参数，所以修改arguments[1]，也就修改了num2，结果他们的值都会变成10，不过，两个值并不会访问相同的内存空间；他们的内存空间是独立的，但它们的值会同步。但这种影响是单向的：修改num2不会改变arguments的值。   

如果只传入一个参数，那么为arguments[1]设置的值不会反应到命名参数中。这是因为arguments对象的长度是由传入的参数个数决定的，并不是由定义函数时命名参数的个数决定的。   

没有值传递的命名参数将自动被赋予undefined值(定义了变量但又没有初始化一样)。  
严格模式对如何使用arguments对象做出了一些限制，首先，像前面例子的复制会变得无效。也就是说，即使把arguments[1]设置为10，num2的值仍然还是undefined。其次，重写arguments的值会导致语法错误。  

## 3.7.2 没有重载  

ECMAScript函数不能像传统意义上那样实现重载。而在其他语言中，只要两个定义的签名不同即刻，但是ECMAScript没有签名。因为其参数是由包含零或多个的值的数组来表示的。没有函数签名，真正的重载是不可能实现的。  

如果在ECMAScript中定义了两个名字相同的函数，则该名字只属于后定义的函数。  


# 4   
在赋值时，解析器必须确定这个值时基本类型值还是引用类型值。  
5种基本数据类型：Undefined、Null、Boolean、Number和String，这5种基本类型是按值访问的，因为可以操作保存在变量中实际的值。  

引用类型的值是保存在内存中的对象。与其他语言不同，JavaScript不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。为此，引用类型是按引用访问的。   


## 4.1.3 传递参数  
ECMAScript中所有函数的参数都是按值传递的。   

为了证明对象是按值传递的:    

```js
function setName(obj) {
    obj.name = "Nicholas";
}

var person = new Object();
setName(person);
alert(person.name); //Nicholas
```


```js
function setName(obj) {
    obj.name = "Nicholas";
    obj = new Object();
    obj.name = "Greg";
}

var person = new Object();
setName(person);
alert(person.name);  //"Nicholas"
```

在函数内部修改了参数的值，但原始的引用仍然保持未变。实际上，在函数内部重写obj时，这个变量引用的就是一个局部对象了。而这个局部对象会在函数执行完毕后立即被销毁。  


## 4.1.4 检测类型   

```js
var s = null;
var o = new Object();

alert(typeof s); //object
alert(typeof o); //object
```

检测引用类型这个操作符用处不大。通常不是想知道某个值是对象，而是想知道它是什么类型的对象。为此，ECMAScript提供了instanceof 操作符：  

```js
result = variable instanceof constructor
```


```js
alert(person instanceof Object);  
``` 

根据规定，所有的引用类型的值都是Object的实例。因此，在检测一个引用类型值和Object构造函数时，instanceof操作符始终会返回true。当然，如果使用instanceof操作符检测基本类型的值，则该操作符始终会返回false，因为基本类型不是对象。  

typeof检测正则表达式时，返回function  


## 4.2 执行环境及作用域   

活动对象最开始时只包含一个变量，即arguments对象(这个对象在全局环境中时不存在的)，作用域链中的下一个变量对象来自包含(外部)环境，而再下一个变量对象则来自下一个包环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。  

标识符解析时沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级逐级地向后回溯，知道找到标识符为止。


## 4.2.1 延长作用域链  

+ try-catch语句的catch块  
+ with语句  

这两个语句都会在作用域链的前端添加一个变量对象。对于with语句来说，会将指定的对象添加到作用域链中。对于catch语句来说，会创建一个新的变量对象，其中包含的是被抛出的错误对象的声明。  

```js
function buildUrl() {
    var qs = "?debug=true";
    with(location) {
        var url = herf + qs;
    }
    return url;
}
```  


with语句内部，定义了一个名为url的变量，因而url就成了函数执行环境的一部分，所以可以作为函数的值被返回。  


## 4.2.2 没有块级作用域  

```js
if (true) {
    var color = "blue";
}
alert(color); // "blue"
```


这里是一个在if语句中定义了变量color。如果是在C、C++或Java中，color会在if语句执行完毕后被销毁。但在JavaScript中，if语句中的变量声明会将变量添加到当前的执行环境。  


1. 声明变量  
   使用var声明的变量会自动被添加到最接近的环境中。在函数内部，最接近的环境就是函数的局部环境；在with语句中，最接近的环境是函数环境。如果初始化变量时没有使用var声明，该变量会自动被添加到全局环境。   
   ```js
   function add(num1, num2) {
       var sum = num1 + num2;
       return sum;
   }
   var result = add(10, 20);
   alert(sum); //报错
   ```





























