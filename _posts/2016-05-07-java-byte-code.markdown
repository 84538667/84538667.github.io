
---
layout: post
title:  java字节码
date:   2016-05-07
categories: java
tags:
- java
- 字节码
---

###分析下java编译后class的字节码

java字节码是java之所以可以跨平台的原因，通过把java编译成字节码，并被各种平台上的jvm虚拟机来进行解释执行字节码(`.class`文件)（jit技术也会使部分字节码被jvm编译执行)。

然后这篇文章就来读一下java的字节码文件.

首先这是这次的java类(jdk版本 1.7.0_79)

``` java
package com.xilaner.bytecode;

public class SimpleTest implements SimpleInterface {
    private String testStr = "hello world";

    @Override
    public void sayHello(String hello) {
        System.out.println(testStr);
    }
}

interface SimpleInterface {
    public void sayHello(String hello);
}

```

javac编译后生成的class文件字节码及各字节分析

这个说明根据后面的格式表格以及javap分析结果对照即可  
里面的 //就是我阅读字节码的注释了，每一部分代表了什么都写了出来

```
//表头
cafe babe  //magic_num
0000  // minor version
0033  //major version

//常量池
0022  //常量池大小 大小－1即33个常量
//第一个常量
0a //类型为method
  0007 //接口描述
  0013 //名称类型描述
//第二个常量
08 //类型为string
  0014 //值索引
//第三个常量
09 //类型为字段符号引用
  0006 //描述
  0015 //名称
//第四个常量
09 //类型为字段符号引用
  0016 //描述
  0017 //名称
//常量 5后面不再一一注释了,自己看下吧。。
0a 
  0018
  0019
//常量 6
07 
  001a
//常量 7 
07
  001b
//常量 8
07 
  001c
//常量 9 
01 
  0007
  7465 7374 5374 72 
//常量 10 
01 
  0012 
  4c6a 6176 612f 6c61 6e67 2f53 7472 696e 673b 
//常量 11 
01 
  0006 
  3c69 6e69 743e 
//常量 12
01 
  0003 
  2829 56 
//常量 13
01 
  0004 
  436f 6465
//常量 14
01 
  000f 
  4c69 6e65 4e75 6d62 6572 5461 626c 65
//常量 15
01 
  0008 
  7361 7948 656c 6c6f 
//常量 16
01 
  0015 
  284c 6a61 7661 2f6c 616e 672f 5374 7269 
  6e67 3b29 56 
//常量 17
01 
  000a 
  536f 7572 6365 4669 6c65
//常量 18
01 
  000f 
  5369 6d70 6c65 5465 7374 2e6a 6176 61
//常量 19
0c 
  000b 
  000c 
//常量 20
01 
  000b 
  6865 6c6c 6f20 776f 726c 64 
//常量 21
0c 
  0009
  000a
//常量 22
07
  001d 
//常量 23
0c
  001e
  001f
//常量 24
07
  0020 
//常量 25
0c
  0021
  0010
//常量 26
01 
  001f 
  636f 6d2f 7869 6c61 6e65 722f 6279 7465
  636f 6465 2f53 696d 706c 6554 6573 74
//常量 27
01 
  0010
  6a61 7661 2f6c 616e 672f 4f62 6a65 6374
//常量 28
01 
  0024 
  636f 6d2f 7869 6c61 6e65 722f 6279 7465 
  636f 6465 2f53 696d 706c 6549 6e74 6572 
  6661 6365
//常量 29
01 
  0010 
  6a61 7661 2f6c 616e 672f 5379 7374 656d 
//常量 30
01 
  0003
  6f75 74
//常量 31
01 
  0015
  4c6a 6176 612f 696f 2f50 7269 6e74 5374
  7265 616d 3b 
//常量 32
01 
  0013 
  6a61 7661 2f69 6f2f 5072 696e 7453 7472
  6561 6d 
//常量 33
01 
  0007
  7072 696e 746c 6e 

//类的属性
0021  //accessFlag
0006  //类
0007  //父类
0001  //接口数量
0008  //接口引用，如果有多个，那么回按照implements的顺序

//字段表
0001  //变量数量
//第一个变量
0002  //变量的access_flag
0009  //字段的名称引用
000a  //字段描述引用
0000  //字段的attribute数量

//方法表
0002  //方法数量
//第一个方法
0001  //access_flag
000b  //方法的名称 
000c  //方法的描述
0001  //方法参数个数
  000d  //属性 : code
  0000 0027  //参数长度
    0002 //max_stack
    0001 //max_locals
    0000 000b //code_length
    2ab7 0001 2a12 02b5 0003 b1 //code
    0000 //异常表长度
    0001 //attritube_count 
      000e //属性:LineNumberTable
      0000 000a //属性长度
      0002 //LineNumberTable长度
        0000 //字节码行号
        0003 //源代码行号 
        0004 //字节码行号
        0004 //源代码行号

//第二个方法
0001 //access_flag
000f //方法名称
0010 //方法描述
0001 //方法参数个数
  000d //属性:code
  0000 0027 //参数长度
  0002 //max_stack 
  0002 //max_locals
  0000 000b //code_length
  b2 0004 2ab4 0003 b600 05b1 
  0000 //异常表长度
  0001 //attritube_count
    000e //属性:LineNumberTable
    0000 000a //属性长度
    0002 //lineNumberTable长度
      //第一个
      0000 //字节码行号
      0008 //源代码行号
      //第二个
      000a //字节码行号
      0009 //源代码行号
      
//class文件属性
0001 //属性数量
  0011 //属性名称: SourceFile
  0000 0002 //属性长度
  0012 //文件名称
```

以上分析纯属闲的蛋疼自己一个字节一个字节读的，反正上面的东西，读完了也什么都记不住。  
好在java提供了一个命令，可以直接分析字节码:

javap分析结果

```
Classfile ~/SimpleTest.class
  Last modified May 4, 2016; size 530 bytes
  MD5 checksum bf201575d8c704f7b6bd57969cd17b9d
  Compiled from "SimpleTest.java"
public class com.xilaner.bytecode.SimpleTest implements com.xilaner.bytecode.SimpleInterface
  SourceFile: "SimpleTest.java"
  minor version: 0
  major version: 51
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #7.#19         //  java/lang/Object."<init>":()V
   #2 = String             #20            //  hello world
   #3 = Fieldref           #6.#21         //  com/xilaner/bytecode/SimpleTest.testStr:Ljava/lang/String;
   #4 = Fieldref           #22.#23        //  java/lang/System.out:Ljava/io/PrintStream;
   #5 = Methodref          #24.#25        //  java/io/PrintStream.println:(Ljava/lang/String;)V
   #6 = Class              #26            //  com/xilaner/bytecode/SimpleTest
   #7 = Class              #27            //  java/lang/Object
   #8 = Class              #28            //  com/xilaner/bytecode/SimpleInterface
   #9 = Utf8               testStr
  #10 = Utf8               Ljava/lang/String;
  #11 = Utf8               <init>
  #12 = Utf8               ()V
  #13 = Utf8               Code
  #14 = Utf8               LineNumberTable
  #15 = Utf8               sayHello
  #16 = Utf8               (Ljava/lang/String;)V
  #17 = Utf8               SourceFile
  #18 = Utf8               SimpleTest.java
  #19 = NameAndType        #11:#12        //  "<init>":()V
  #20 = Utf8               hello world
  #21 = NameAndType        #9:#10         //  testStr:Ljava/lang/String;
  #22 = Class              #29            //  java/lang/System
  #23 = NameAndType        #30:#31        //  out:Ljava/io/PrintStream;
  #24 = Class              #32            //  java/io/PrintStream
  #25 = NameAndType        #33:#16        //  println:(Ljava/lang/String;)V
  #26 = Utf8               com/xilaner/bytecode/SimpleTest
  #27 = Utf8               java/lang/Object
  #28 = Utf8               com/xilaner/bytecode/SimpleInterface
  #29 = Utf8               java/lang/System
  #30 = Utf8               out
  #31 = Utf8               Ljava/io/PrintStream;
  #32 = Utf8               java/io/PrintStream
  #33 = Utf8               println
{
  public com.xilaner.bytecode.SimpleTest();
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0       
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0       
         5: ldc           #2                  // String hello world
         7: putfield      #3                  // Field testStr:Ljava/lang/String;
        10: return        
      LineNumberTable:
        line 3: 0
        line 4: 4

  public void sayHello(java.lang.String);
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=2, args_size=2
         0: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: aload_0       
         4: getfield      #3                  // Field testStr:Ljava/lang/String;
         7: invokevirtual #5                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        10: return        
      LineNumberTable:
        line 8: 0
        line 9: 10
}
```

后面是这个文件中用到的类文件结构，字节码的注释就是通过这个一个一个字节读的  
### 类文件的结构如下

#### 总体结构:

类型 | 名称 | 数量
--- | --- | ---
u4 | magic | 1
u2 | minor_version | 1
u2 | major_version | 1
u2 | constant\_pool_count | 1
cp_info | constant_pool | constant\_pool_count - 1
u2 | access_flags | 1
u2 | this_class | 1
u2 | super_class | 1
u2 | interfaces_count | 1
u2 | interfaces | 1
u2 | fields_count | 1
field_info | fields | fileds_count
u2 | methods_count | 1
method_info |methods  | mthods_count
u2 | attritubes_count | 1
attrirube_info| attritubes | attritubes_count


#### 常量池

类型 | 标志 | 描述
---|---|---
CONSTANT\_Utf8_info | 1 | UTF-8编码的字符串
CONSTANT\_Integer_info | 3 | 整形字面量
CONSTANT\_Float_info | 4 | 浮点型形字面量
CONSTANT\_Long_info | 5 | 长整形字面量
CONSTANT\_Double_info | 6 | 双精度浮点型字面量
CONSTANT\_Class_info | 7 | 类和接口的符号引用
CONSTANT\_String_info | 8 | 字符串类型字面量
CONSTANT\_FieldRef_info | 9 | 子段的符号引用
CONSTANT\_Methodref_info | 10 | 类中方法的符号引用
CONSTANT\_InterfaceMethodref_info | 11 | 借口中方法的符号引用
CONSTANT\_NameAndType_info | 12 | 字段或方法的部分符号应用

其中每一类型的接口如下:


常量 | 名称 | 类型 | 描述
---|---|---|---
CONSTANT\_Utf8_info| tag | u1 | 值为1 
  | length | u2 | UF-8编码的字符串占用的字节数 
  | bytes | u1| 长度为length的UTF-8编码的字符串 
CONSTANT\_Integer_info | tag | u1 | 值为3
  | bytes | u4 | 按照高位在前存储的int值
CONSTANT\_Float_info | tag | u1 | 值为4
  | bytes | u4 | 按照高位在前存储的float值
CONSTANT\_Long_info | tag | u1 | 值为5
  | bytes | u8 | 按照高位在前存储的long值
CONSTANT\_Double_info  | tag | u1 | 值为6
  | bytes | u8 | 按照高位在前存储的double值
CONSTANT\_Class_info | tag | u1 | 值为7
  | index | u2 | 指向全限定名常量项的索引
CONSTANT\_String_info | tag | u1 | 值为8
  | index | u2 | 指向字符串字面量的索引
CONSTANT\_FieldRef_info | tag | u1 | 值为9
  | index | u2 | 指向声明字段的类或接口描述符CONSTANT_Class_info的索引项
  | index | u2 | 指向字段名称及类型描述符CONSTANT_NameAndType_info的索引项
CONSTANT\_Methodref_info | tag | u1 | 值为10
  | index | u2 | 指向声明方法的类描述符CONSTANT_Class_info的索引项
  | index | u2 | 指向方法名称及类型描述符CONSTANT_NameAndType_info的索引项
CONSTANT\_InterfaceMethodref_info | tag | u1 | 值为11
  | index | u2 | 指向声明方法的类描述符CONSTANT_Class_info的索引项
  | index | u2 | 指向方法名称及类型描述符CONSTANT_NameAndType_info的索引项
CONSTANT\_NameAndType_info | tag | u1 | 值为12
  | index | u2 | 指向字段或方法名称常量项目的索引
  | index | u2 | 指向该字段或方法描述符常量项的索引
  
#### access_flag

名称  |值 | 描述
---|---|---
ACC_PUBLIC | 0x0001 | public
ACC_PRIVATE | 0x0002 | private
ACC_PROTECTED | 0x0004 | protected
ACC_STATIC | 0x0008 | static
ACC_FINAL | 0x0100 | final
ACC_VOLATILE | 0x0400 | volatile
ACC_TRANSIENT | 0x0800 | transient
ACC_SYNTHETIC | 0x1000 | synthetic
ACC_ENUM | 0x4000 | enum

#### 字段表

类型 | 名称 | 数量
---|---|---
u2 | access_flag | 1
u2 | name_index | 1
u2 | description_index | 1
u2 | attritubes_count | 1
attritube_info | attritube | attritubes_count

#### 方法表

类型 | 名称 | 数量
---|---|---
u2 | access_flag | 1
u2 | name_index | 1
u2 | description_index | 1
u2 | attritubes_count | 1
attritube_info | attritube | attritubes_count


#### 本例中属性表

属性名称|使用位置|含义
---|---|---
Code|方法表|Java代码编译成的字节码指令
Exceptions|方法表 |方法抛出的异常
LineNumberTable|Code属性 |Java源码的行号与字节码指令的对应关系 
SourceFile|类文件|记录源文件名称 

jvm预置了n多属性，由于没有用到，就不一一介绍了(主要我也不知道)，需要了解的可以自行google
   
##### code 属性

类型|名称|数量
---|---|---
u2|attribute_name_index|1
u4|attribute_length|1
u2|max_stack|1
u2|max_locals|1
u4|code_length|1
u1|code|code_length
u2|exception_table_length|1
exception_info|exception_table|exception_length
u2|attributes_count|1
attribute_info|attributes|attributes_count

>max_stack表示最大栈深度，虚拟机在运行时根据这个值来分配栈帧中操作数的深度  
max_locals代表了局部变量表的存储空间。  
max_locals的单位为slot，slot是虚拟机为局部变量分配内存的最小单元，在运行时，对于不超过32位类型的数据类型，比如 byte,char,int等占用1个slot，而double和Long这种64位的数据类型则需要分配2个slot，另外max_locals的值并 不是所有局部变量所需要的内存数量之和，因为slot是可以重用的，当局部变量超过了它的作用域以后，局部变量所占用的slot就会被重用。

##### exception 属性

类型|名称|数量
---|---|---
u2|attribute\_name_index|1
u2|attribute_length|1
u2|attribute_of\_exception|1
u2|exception\_index_tsble|number_of_exceptions


##### LINENUMBERTABLE 属性

类型|名称|数量
---|---|---
u2|attribute_name_index|1
u4|attribute_length|1
u2|line_number\_table_length|1
line_number\_info|line_number\_table|line\_number_table_length

##### sourceFile 属性

类型|名称|数量
---|---|---
u2|attribute_name_index|1
u4|attribute_length|1
u2|sourcefile_index|1
