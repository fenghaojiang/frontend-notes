---
title: JavaScript高级程序设计笔记2
date: 2021-06-30
---  

## 4.3 垃圾回收  

局部变量只在函数执行的过程中存在。而在这个过程中，会为局部变量在栈(或堆)内存上分配相应的空间以便存储他们的值。然后在函数中使用这些变量，直至函数执行结束。此时，局部变量就没有存在的必要；但并非所有情况下都这么容易就能得出结论。  


### 4.3.1 标记清除(mark-and-sweep)  

当变量进入环境(函数中声明一个变量)时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。而当变量离开环境时，则将其标记成“离开环境”  
垃圾收集器在运行时的时候会给存储在内存中的所有变量都加上标记(当然，可以使用任何标记方式)。  
然后，他会去掉环境中的变量以及被环境中变量引用的变量的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾收集器完成内存清除工作，销毁那些带标记的值并回收他们所占用的内存空间。   


### 4.3.2 引用计数(reference counting)  

引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型的值赋给该变量时，则这个值的引用次数就是1  
如果同一个值又被赋给另一个变量，则这个值的引用次数减1  
当这个值得引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾收集器下次再次运行时，他就会释放那些引用次数为零的值所占用的内存  

```js
function problem() {
    var obj1 = new Object();
    var obj2 = new Object();
    obj1.someOtherObject = obj2;
    obj2.anotherObject = obj1;
}
```  
两个对象的引用次数都是2。在采用标记清除的策略实现中，由于函数执行之后，这两个对象都离开了作用域，因此这种相互引用不是个问题。  

但在引用计数策略实现中，当函数执行完毕后，objectA和objectB还将继续存在，因为他们的引用次数永远不会是0  
假如这个函数被重复多次调用，就会导致大量内存得不到回收。    

只要IE中涉及COM(Component Object Model, 组件对象模型)对象， 就会存在循环引用的问题。    

为了解决上述问题，IE9把BOM和DOM对象都转换成了真正的JavaScript对象。这样，就避免了两种垃圾收集算法并存导致的问题，也消除了常见的内存设计问题。 


## 5 引用类型  

### 5.1 Object类型  

```js
alert(person["name"]);
alert(person.name);
```
从功能上看，这两种访问对象属性的方法没有任何区别。但方括号语法的主要优点是可以通过变量来访问属性  


### 5.2.4 队列方法   

```js
var colors = new Array();
var count = colors.push("red", "green");
alert(count); // 2


count = color.push("black");
alert(count);  // 3

var item = colors.shift(); //移除第一项
alert(item); // "red"
alert(colors.lenght); //2
```  

ECMAScript还为数组提供了一个unshift()方法。顾名思义，unshift()和shift()的用途相反：它能在数组的前端添加任意个项并返回新数组的长度。因此，同时使用unshift()和pop()方法，可以从相反的方向来模拟队列，即再数组的前端添加项，从数组末端移除项，如下面的例子所示。  

```js
var colors = new Array();
var count = colors.unshift("red", "green");
alert(count); //2

count = colors.unshift("block");
alert(count); // 3

var item = colors.pop();
alert(item);  // "green"
alert(colors.length); //2
```

slice()方法  


splice()方法---最强大的数组方法，它有很多种用法。主要用途是向数组的中部插入项。  

+ 删除: 可以删除任意数量的项，只需要指定两个参数：要删除的第一项的位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项。  
+ 插入: 可以向指定位置插入任意数量的项，只需提供3个参数：起始位置、0(要删除的项数)和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0, "red", "green")会从当前数组的位置2开始插入字符串"red"和"green"  
+ 替换: 可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需要指定3个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice(2, 1, "red", "green")会删除当前数组位置2的项，然后再从位置2开始插入字符串"red"和"green"  

splice()方法始终都会返回一个数组，该数组的中包含从原始数组中删除的项(如果没有删除任何项，则返回一个空数组)  

```js
var colors = ["red", "green", "blue"];
var removed = colors.splice(0, 1);
alert(colors); //green, blue
alert(removed); //red

removed = colors.splice(1, 0, "yellow", "orange");
alert(colors); //green, yellow, orange, blue
alert(removed); // 返回的是一个空数组

removed = colors.splice(1, 1, "red", "purple");
alert(colors);
alert(removed);
```  


