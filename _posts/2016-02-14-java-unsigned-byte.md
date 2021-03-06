---
layout: post
title: Java Unsigned Byte
categories: java
comments: true
tags: java
date: 2016-02-14 16:00
---

Java 中不存在无符号的基础数据类型，这个规定和 C 语言相比似乎简化了许多。然而在使用过程中却也有令人疑惑的地方。

```java
byte b = (byte)200;
int i = b; // 此时 i 的值是 -56
```

如果你不注意，在一个复杂的程序中写上这样一段代码，想当然得以为 i 的值也是 200，之后寻找这样的一个隐蔽的 bug 也是比较麻烦的。

上面问题的实质是：当我们把 b 赋值给 i 的时候，b 的最高位会展开填充 i 的高位。

```
b                                1100 1000                                
i  1111 1111 1111 1111 1111 1111 1100 1000 // -56
```

那么如何把一个字节当成无符号的整数呢？

```java
byte b = (byte)200;
int i = b & 0xFF; // 此时 i 的值就是 200
```

上面做法的原理如下

```
1111 1111 1111 1111 1111 1111 1100 1000 // b 会先转换成 int 类型
&
0000 0000 0000 0000 0000 0000 1111 1111 // 这是 0xFF 的值
=======================================
0000 0000 0000 0000 0000 0000 1100 1000 // 结果 200
```

结束语：我个人觉得在实际使用过程中把 byte 类型数据当成有符号的整数的情况非常少，大多数情况下都需要将其看做 0~255 这样一个无符号数值，所以当你把 byte 类型的变量赋值给其他任何非 byte 类型的变量的时候，别忘了加上 `byte & 0xFF`

## 参考
1. [Can we make unsigned byte in Java](http://stackoverflow.com/questions/4266756/can-we-make-unsigned-byte-in-java)
