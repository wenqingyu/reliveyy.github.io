---
layout: post
title: Java 复习一：语言基础
categories: java
comments: true
tags: [java]
date: 2016-02-17 21:26
---

## 语法

从字面上看任何 Java 程序都是由下面七类字符（字符串）组成。

* Keywords（关键字）
* Identifiers（标识符）
* Operators（操作符）
* Literals（字面值）
* Comments（注释）
* Delimiters（定界符）：大括号，中括号，小括号，尖括号
* Whitespaces（空白符）：空格符（`SP`），水平制表符（`HT`），换行（`CR`，`LF`），分页符（`FF`）

### 关键字

Java 一共有 50 个关键字，简单分个类：

* import，package
* class，**enum**，extends，implements，interface，abstract
* public，private，protected
* void，boolean，byte，short，int，long，float，double，char，**strictfp**
* for，while，do，break，continue，goto
* if，else，switch，case，default
* this，super
* throws，return
* final，**const**，static，transient，native
* try，catch，finally，throw
* synchronized，volatile
* new，instanceof，assert
* null，false，true

`enum` 是类，和 C++ 不同；`strictfp` 是确保在不同的系统上浮点数完全相同；`const` 虽然是关键字，但是却没有任何功能，请使用 `final` 定义常量。

```java
strictfp class Foo{}
strictfp interface Foo{}
strictfp void method(){}
strictfp abstract void method(){} // 错，虚方法
class Foo{strctfp Foo(){}}        // 错，构造方法
strictfp float a = 3.14;          // 错
```

Java 中的共有 11 个修饰符（Modifiers），并没有强制的顺序规定，但是《Java 语言规范》当中有一个推荐的顺序：public，protected，private，abstract，static final，transient，volatile，synchronized，native，strictfp

### 标识符

* 区分大小写
* 第一个字符：a-z，A-Z，**$**，_
* 后面的字符随意，只要不是空白符就行
* 用途：变量（属性）名，函数（方法）名，类名，接口名

```java
long $a   = 2147483647;    	 // 正确
long b变量 = 2147483648L;    // 正确
```

### 操作符

优先级不确定的时候用括号，Java 中没有运算符重载。

| 类型 		| 优先级依次递减						|
|:---------:|----------------------------------|
|后置		| `expr++` `expr--`					|
|一元		| `++expr` `--expr` `+expr` `-expr` `~` `!` |
|乘除取余 	| `*` `/` `%`						|
|加减		| `+` `-`							|
|移位	   	| `<<` `>>` `>>>`					|
|关系		| `<` `>` `<=` `>=` `instanceof`	|
|等于		| `==` `!=`							|
|位与		| `&`								|
|异或		| `^`								|
|位或		| `|`								|
|逻辑与		| `&&`								|
|逻辑或		| `||`								|
|三元		| `? :`								|
|赋值  		| `=` `+=` `-=` `*=` `/=` `%=` `&=` `^=` `|=` `<<=` `>>=` `>>>=` |

#### 闰年

```java
y % 4 == 0 && y % 100 != 0 || y % 400 == 0
```


#### 位运算

| 类型 	| 位运算符 	| 逻辑运算符/备注 	|
|---|---|---|
| 与			| `&`	| `&&`	|
| 或			| `|`	| `||`	|
| 非	（取反）	| `~`	| `!`	|
| 异或		| `^`	| -1 异或的结果是 0	|
| 左移		| `<<`	| 补 0				|
| 右移		| `>>`	| 根据符号位			|
| 右移		| `>>>`	| 补 0				|

* **原码**：符号位 + 真实值
* **反码**：正数不变，负数原码取反
* **补码**：正数不变，负数原码取反加一
* 用移位快速乘除 2 的倍数
* `& 1` 判断奇偶
* `byte & 0xFF` 获得 0 ~ 255 的 int 值
* flag，mask
* `x^=y; y^=x; x^=y;`
* `0x5f3759df` 快速计算 x^(-1/2)

```
符号位
  1    011 1000 // 原码 -56
  1    100 0111 // 反码 -56
  1    100 1000 // 补码 -56
```

#### instanceof

```java
x instanceof java.lang.String   // 类
x instanceof java.lang.Runnable // 接口
x instanceof int[]              // 数组

private <T> void method(Object x) {
    if (!(x instanceof T)) return; // 错
}
// 可改写成
private <T> method(T x) {}
```

### 字面值（Literals）

#### 整数

* 写在程序里的整数都被编译器当做 `int`
* 如果在 -2147483648 ~ 2147483647 以外的整数请在数字末尾加上 `L` 或者 `l`，指明这个数字是 `long`
* 二进制：`0b` 或者 `0B` 开头
* 八进制：`0` 开头，IntelliJ IDEA 默认会提示你把八进制数字转换成十进制
* 十六进制：`0x` 或者 `0X` 开头
* 数字太长可以用下划线分割：`0x123_456_789L`

#### 浮点数

* 写在程序里的带小数的数字都被编译器当做 `double`，
* 在数字后面加上 `D` 或者 `d`，指明这个数字是 `double`
* 在数字后面加上 `f` 或者 `F`，指明这个数字是 `float`
* `0.2` 也可以写作 `.2`
* `.2e-6` `.2E-6` 表示 0.2 &times; 10^(-6)
* `.2e+6`

#### 字符

* UTF-8 的源代码里可以用单引号括起来任意一个 Unicode 字符 `'❤'` 来表示一个**字符**
* `'\u2764'` 和 `'❤'` 效果一样，注意 u 不能大写，后面必须跟 4 个十六进制数字
* `'\177'` 八进制字符，最多 3 个数字；3 个数字的时候第一位只能是 0 - 3
* 当时 Java 设计的时候认为 2 个字节的 Unicode 字符已经足够，万万没想到现在 Unicode 已经突破 2 个字节了，这一类字符叫 [增补字符（Supplementary Characters）](http://www.oracle.com/us/technologies/java/supplementary-142654.html)（`10000 - 10FFFF` 目前 Unicode 最长的字符有 3 个字节，Java 的解决方案是使用两个 char 来表示一个增补字符）

```java
char[] chars = Character.toChars(0x29D98); 
// High Surrogate: 0xD867
// Low  Surrogate: 0xDD98
// 这是 UTF-16 的表示方法，不要简单得以为 0x29D98 
// 拆分成两个 char 后就是 0x0002 和 0x9D98
String s = new String(chars);
```

| 转义字符	| 含义 |
|---|---|
| `\b`		| `BS` 退格符		|
| `\t`		| `HT` 水平制表符	|
| `\n`		| `LF` 换行符		|
| `\f`		| `FF` 换页符		|
| `\r`		| `CR` 回车符		|
| `\"`		| 双引号			|
| `\'`		| 单引号			|
| `\\`		| 反斜杠			|

**注意**：Java 中没有续行符，即没有 `\<LF>` 这种转义字符

#### 字符串

* `"text"` 会使用字符串常量池（String Constant Pool）中已有的 String 实例
* 字符串内可以使用**转义字符**
* `"abc" + "123"` 将一个长的字符串分割成两行；这种写法和一行没有任何区别，编译器会优化

```java
String a = "abc";
String b = "abc";
System.out.println(a == b); // true

String a = new String("abc");
String b = new String("abc");
System.out.println(a == b); // false
```

### 注释

* `// comments`
* `/* comments */`
* `/** javadoc */`
* 不嵌套
* IntelliJ IDEA 中自定义代码折叠区域 
	* NetBeans 风格
	  
	  ```
	  //<editor-fold desc="Description">
      ...
      //</editor-fold>
	  ```
	* Visual Studio 风格
	  
	  ```
	  //region Description
      ...
      //endregion
	  ```

## 变量

一个变量的实质就是一段内存区域，当里面存储的是内存地址的时候，它就是引用（指针）

### 局部变量

* 在一对大括号内部（类的方法中，静态初始化代码块）
* 可以声明为 final
* 不能声明为 static，本身也没有任何语义
* 存储于 JVM 的 Stack 中，是 Stack Frame 的一部分

### 属性

* 实例属性：非 static 属性，可声明为 final
* 类属性：static 属性，可声明为 final

### 方法的参数

* 传值（value）：基础数据类型
* 传引用（reference）：指向一个实例的变量
* 可以声明为 final，表示这个参数变量在方法内部不会被赋值；假如变量是一个引用，final 的意义就不是很大。

### 全局变量

* Java 中没有全局变量
* 可以使用类属性代替

### 引用

* 当一个实例没有任何**强引用**指向它的时候，它就会被标记上**可回收**
* 强引用（Strong Reference）：默认，活到 JVM 关闭，除非手动设置为 null
* 软引用（Soft Reference）：内存吃紧时被回收，常用于缓存
* 弱引用（Weak Reference）：垃圾回收线程扫描到就回收，常用于默默围观，可以有效避免循环引用
* 虚引用（Phantom Reference）：最弱的引用，以至于 `get()` 永远返回 null，必须在创建的时候指定一个 ReferenceQueue，它的作用就是探测它指向的对象是否被回收了
* 引用队列（ReferenceQueue）：垃圾回收后将软引用，弱引用，虚引用放入其中


### 作用域

* 一对大括号就创建了一个作用域，可嵌套

```java
{
    {int a = 1;}
    a = 2; // 错，本作用域中没有定义变量 a
}
```

## 代码

* 表达式（Expression）：一段代码，最终结果是一个值
* 语句（Statement）：以分号结束；分支，循环，声明，赋值
* 代码块（Code Block）：多条语句

```java
if (conditions) {...} else {...}

if (conditions) {...}
else if (conditions) {...}
else {...}

switch (char/byte/short/int/String/enum) {
    case value:
        ...
        break;
    default:
        ...
}

for (int i = 0; i < 10; i++) {...}
for (Object x: iterable) {...}

while (conditions) {...}
do {...} while (conditions);
```
