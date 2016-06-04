---
layout: post
title:  闭包
date:   2016-06-03
categories: js
tags:
- js
- 闭包
---

#### 1.闭包（closure）：定义一大堆，大概理解一下就是，一个函数内部提供了可以查看函数内部属性的方法，这个方法加上函数的内部属性就是一个闭包

举个栗子：

~~~
　　function func(){
　　　　var innervalue=1;
　　　　function innerfunc(){
　　　　　　return innervalue;
　　　　}
　　　　return innerfunc;
　　}
　　var f = func();
　　console.log(f());
~~~

这段代码应该很容易看懂吧,输出应该是1，我们通过`func`内的`innerfunc`拿到了func里面的变量`innervalue`，而原本`innervalue`这个值只能在函数内部读取，这就是__闭包__

#### 2.私有化成员

~~~
var xiaoming = (function(){
  var name = "xiaoming";
  var age = 180000;
  function getName(){
    return name;
  }
  function getAge(){
    return age;
  }
  function setAge(_age){
    age = _age;
  }
  return {
    getName:getName,             
    getAge:getAge,
    setAge:setAge
  }
})();

console.log(xiaoming.getName());  //xiaoming
console.log(xiaoming.getAge());   //180000
xiaoming.setAge(190000);        
console.log(xiaoming.getAge());   //190000   

~~~

通过闭包可以将一个对象的成员变量私有化，只有通过方法可以获取。这是一种主要用法。

#### 3.前段组件遍历的问题

前段会遇到这样的问题:又一个列表，点击上面的不同元素时，对对应的元素进行某一个操作，直接使用for循环的写法:

~~~
function(){
  var lis = document.getElementsByTagName('li');
  for (var i=0;i<lis.length;i++){           
    lis[i].onclick = function(){       
      alert(i);       
    };
  }
}()
~~~

但是实际运行时会发现，使用的时候永远都是最后一个控件被触发了事件。上面的代码，如果
lis的长度为5，那么不管点击哪一个li都显示4。这是因为实际点击组件时，for循环已经运行完，而其中的i时共享的，所有所有的i都为4。

解决方案:使用闭包,从而使内部的i变为局部变量。

~~~
function(){
  var lis = document.getElementsByTagName('li');
  for (var i=0;i<lis.length;i++){           
    function(i){
      lis[i].onclick = function(){       
	     alert(i);       
	   };
    }(i);
  }
}()
~~~

#### 4.问题 

闭包使用不当会造成严重的__内存浪费__:从上面的例子可以看出来，如果`xiaoming`这个对象不被释放，那么小明内部全部的属性都会保存在内存中，不会被回收。如果大量使用但是没有释放的话，会造成大量的内存浪费，使用不当容易造成内存泄漏。

