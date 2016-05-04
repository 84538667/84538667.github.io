---
layout: post
title: 内部类
date: 2016-05-04
categories: java
tags:
- java
- 内部类
---

内部类是指在一个外部类的内部再定义一个类。内部类作为外部类的一个成员，并且依附于外部类而存在的。  
内部类可为静态，可用protected和private修饰（而外部类只能使用public和缺省的包访问权限）。  
内部类是个编译时的概念，一旦编译成功后，它就与外围类属于两个完全不同的类。对于一个名为OuterClass的外围类和一个名为InnerClass的内部类，在编译成功后，会出现这样两个class文件：OuterClass.class和OuterClass$InnerClass.class  
内部类主要有以下几类：成员内部类、局部内部类、匿名内部类、静态内部类

<!-- more -->

##### 1.成员内部类

成员内部类：是外围类的一个成员

``` java
package com.xilaner.innerClass;

class OuterClass {

	 //后面两个例子会把这10个outerclass的方法和属性隐藏掉，太占地方
    public static String outerStaticStr    = "outer static str";

    public String        outerPublicStr    = "outer public str";

    String               outerPackageStr   = "outer package str";

    protected String     outerProtectedStr = "outer protected str";

    private String       outerPrivateStr   = "outer private str";

    public void outerPublicMethod() {
        System.out.println("outer public method");
    }

    void outerPackageMethod() {
        System.out.println("outer package method");
    }

    protected void outerProtectedMethod() {
        System.out.println("outer protected method");
    }

    private void outerPrivateMethod() {
        System.out.println("outer private method");
    }

    @Override
    public String toString() {
        return "outer class";
    }

    public class InnerClass {
        public void test() {
            //可以直接获取外部类的各种属性
            System.out.println(outerStaticStr);
            System.out.println(outerPublicStr);
            System.out.println(outerPackageStr);
            System.out.println(outerProtectedStr);
            System.out.println(outerPrivateStr);
            outerPublicMethod();
            outerPackageMethod();
            outerProtectedMethod();
            outerPrivateMethod();
            System.out.println(OuterClass.this.toString());
        }

        private void call() {
            System.out.println("inner");
        }
    }

    public void outerCallInner() {
        //外部类使用内部类需要先实例化
        InnerClass inner = new InnerClass();
        inner.call();
    }

    public static void outerStaticCallInner() {
        //静态方法对应非静态内部类需要先实例化外部类
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.call();
    }
}

public class InnerClassTest {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.test();
        System.out.println("----------------");
        outer.outerCallInner();
        System.out.println("----------------");
        OuterClass.outerStaticCallInner();
    }
}
```
输出:

```
outer static str
outer public str
outer package str
outer protected str
outer private str
outer public method
outer package method
outer protected method
outer private method
outer class
----------------
inner
----------------
inner
```

- 成员内部类中不能存在任何static的变量和方法
- 成员内部类是依附于外围类的，所以只有先创建了外围类才能够创建内部类
- 可以访问外部类全部属性方法
- 成员内部类可以使用 protected和private修饰符来进行修饰已修改其访问权限
- 内部类和外部类属性重名时，通过`this.xxx`和`OuterClass.this.xxx`进行区分即可，没有前缀默认使用的是`this.xxx`
- 外部类需要访问内部类时，需要先实例化
- 外部类可以访问内部类的private属性
- 静态方法访问时与外部访问一致，需要先实例化外部类
- 这里可以通过`getInnerClass`返回一个内部类的实例，即使内部类为private，但是外围调用该方法会编译错误。

##### 2.局部内部类

局部内部类: 在方法中定义的内部类称，局部内部类和成员内部类一样被编译，只是它的作用域发生了改变，它只能在该方法和属性中被使用，出了该方法和属性就会失效。

``` java
package com.xilaner.innerClass;

class OuterClass {

    ...
	
    public void innerClass() {
        //必须为final，否则不能访问
        final String outerfinalStr = "outer str";
        class InnerClass {
            private void test() {
                System.out.println(outerStaticStr);
                System.out.println(outerPublicStr);
                System.out.println(outerPackageStr);
                System.out.println(outerProtectedStr);
                System.out.println(outerPrivateStr);
                outerPublicMethod();
                outerPackageMethod();
                outerProtectedMethod();
                outerPrivateMethod();
                System.out.println(OuterClass.this.toString());
                System.out.println(outerfinalStr);
            }
        }

        InnerClass inner = new InnerClass();
        inner.test();
    }
}

public class InnerClassTest {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        outer.innerClass();
    }
}
```

结果

```
outer static str
outer public str
outer package str
outer protected str
outer private str
outer public method
outer package method
outer protected method
outer private method
outer class
outer str
```

- 局部内部类中不能存在任何static的变量和方法
- 局部内部类无法在外围实例化
- 内部类和外部类属性重名时，通过`this.xxx`和`OuterClass.this.xxx`进行区分即可，没有前缀默认使用的是`this.xxx`
- 可以访问外部类的全部属性方法，可以访问局部定义的属性，但必须是final的

    >因为虽然匿名内部类在方法的内部，但实际编译的时候，内部类编译成Outer.Inner,这说明内部类所处的位置和外部类中的方法处在同一个等级上，外部类中的方法中的变量或参数只是方法的局部变量，这些变量或参数的作用域只在这个方法内部有效。方法结束后，变量不存在导致内部类引用变量非法。  
    若定义为final，则java编译器则会在内部类内生成一个外部变量的拷贝，而且可以既可以保证内部类可以引用外部属性，又能保证值的唯一性。
若不定义为final，则无法通过编译，因为编译器不会给非final变量进行拷贝，那么内部类引用的变量就是非法的！

- 这里可以通过`getInnerClass`返回一个内部类的实例，但是外围调用该方法会编译错误。
- 外部可以访问内部类的private属性

##### 3.匿名内部类

匿名内部类就是没有名字的内部类。

``` java
package com.xilaner.innerClass;

class OuterClass {

    ...
	
    public InnerClass innerClass() {
        //必须为final，否则不能访问
        final String outerfinalStr = "outer str";
        return new InnerClass() {
            @Override
            public void test() {
                System.out.println(outerStaticStr);
                System.out.println(outerPublicStr);
                System.out.println(outerPackageStr);
                System.out.println(outerProtectedStr);
                System.out.println(outerPrivateStr);
                outerPublicMethod();
                outerPackageMethod();
                outerProtectedMethod();
                outerPrivateMethod();
                System.out.println(OuterClass.this.toString());
                System.out.println(outerfinalStr);
            }
        };

    }
}

interface InnerClass {
    void test();
}

public class InnerClassTest {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        outer.innerClass().test();
    }
}
```

输出

```
outer static str
outer public str
outer package str
outer protected str
outer private str
outer public method
outer package method
outer protected method
outer private method
outer class
outer str
```

- 匿名内部类为局部内部类，所以局部内部类的所有限制同样对匿名内部类生效。
- 使用匿名内部类时，我们必须是继承一个类或者实现一个接口，但是两者不可兼得，同时也只能继承一个类或者实现一个接口。
- 匿名内部类中是不能定义构造函数的。
- 匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法。
- 初始化参数，以final值传递并在匿名类中定义属性赋值即可。
- abstract类的匿名类初始化，因为abstract class可以拥有构造函数，所以直接传递参数即可，但是该构造函数无法被覆盖。

``` java
package com.xilaner.innerClass;

//静态块初始化
class OuterClass {

    public InnerClass innerClass(final int age) {
        return new InnerClass() {
            private int innerage;
            {
                if (age > 10) {
                    this.innerage = age;
                } else {
                    this.innerage = 10;
                }
            }
            @Override
            public void test() {
                System.out.println(innerage);
            }
        };

    }
}

interface InnerClass {
    void test();
}

public class InnerClassTest {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        outer.innerClass(100).test();
        outer.innerClass(9).test();
    }
}
```
``` java

//abstract class初始化
class OuterClass {
    public InnerClass innerClass(String test) {
        //必须为final，否则不能访问
        return new InnerClass(test) {
            @Override
            public void test() {
                System.out.println(innerStr);
            }
        };

    }
}

abstract class InnerClass {
    String innerStr;
    InnerClass(String s) {
        innerStr = s;
    }
    abstract void test();
}


public class InnerClassTest {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        outer.innerClass("test").test();
    }
}
```

__匿名内部类的一个特殊用法__，双括号初始化:

``` java
List list = new ArrayList<String>() {//这个括号定义了一个匿名内部类
    //内部类的初始化块
    {
        add("123");
    }
};
```

- 使用这种方式可以初始化一个list值并且为其初始化一个值。但是注意一点，此时list的类型已经不再是ArrayList类了，而是当前方法中的一个匿名内部类，继承了ArrayList。  
- 这种方式可以用类初始化除final类之外的任何类，因为final类不能被继承  
- ___注意：___这种初始化会形成一个新的类，可能会引起equals等方法判断类型时不一致而导致比较失败，如果需要使用equals时，请注意这种情况的兼容，否则不要使用这种方式初始化

上述代码执行结果与下面代码一致

``` java
class InnerArrayList extends ArrayList {//新的局部内部类
    //初始化块
    {
        add("123");
    }
}

List list = new InnerArrayList();
```

##### 4.静态内部类

静态内部类(嵌套内部类): 声明为static的内部类

``` java
package com.xilaner.innerClass;

class OuterClass {

    public static String  outerPublicStaticStr  = "outer public static str";

    private static String outerPrivateStaticStr = "outer private static str";

    public static void outerPublicStaticMethod() {
        System.out.println("outer public static");
    }

    private static void outerPricateStaticMethod() {
        System.out.println("outer private static");
    }

    public static class InnerClass {
        private static String innerPublicStr = "inner public static str";

        public void test() {
            System.out.println(innerPublicStr);
            System.out.println(outerPublicStaticStr);
            System.out.println(outerPrivateStaticStr);
            outerPublicStaticMethod();
            outerPricateStaticMethod();
        }

        public void call() {
            System.out.println("inner");
        }

        private static void staticCall() {
            System.out.println("static inner");
        }
    }

    public void outerCallInner() {
        InnerClass.staticCall();
        //外部类使用内部类需要先实例化
        InnerClass inner = new InnerClass();
        inner.call();
    }

    public static void outerStaticCallInner() {
        OuterClass.InnerClass inner = new OuterClass.InnerClass();
        inner.call();
    }
}

public class InnerClassTest {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = new OuterClass.InnerClass();
        inner.test();
        System.out.println("----------------");
        outer.outerCallInner();
        System.out.println("----------------");
        OuterClass.outerStaticCallInner();
    }
}
```

输出

```
inner public static str
outer public static str
outer private static str
outer public static
outer private static
----------------
static inner
inner
----------------
inner
```

- 非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围内，但是静态内部类却没有
- 要创建静态内部类的对象，并不需要其外围类的对象。
- 不能从静态内部类的对象中访问非静态的外围类对象。
- 与成员内部类相比，可以创建static属性和方法。


> 静态嵌套类可以理解为一个为了打包方便而被嵌套进外部类的顶级类，与外部类是相互独立的。


##### 内部类使用场景
- 如果一个类只在一个地方被使用，同时这个类和另外一个类在逻辑上紧密相关，那么这个类可以被设计成内部类。
- 增强了封装性
- 嵌套类增加代码的可读性和可维护性（因为嵌套类从逻辑上来说都是相关的）。
