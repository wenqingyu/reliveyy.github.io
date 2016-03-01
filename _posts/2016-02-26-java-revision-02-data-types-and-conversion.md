---
layout: post
title: Java 复习二：基本数据类型及转换
categories: java
comments: true
tags: [java]
date: 2016-02-26 17:25
---

## 基础数据类型

|名称|长度|范围|
|---|:-:|---|
|byte	|1| -128 ~ 127 						|
|short	|2| -32768 ~ 32767 					|
|int	|4| -21,4748,3648 ~ 21,4748,3647 	|
|long	|8| -922,3372,0368,5477,5808 ~ 922,3372,0368,5477,5807 |
|char	|4| 0 ~ 65535 						|
|float	|4|	±1.40129846432481707e-45 ~ ±3.40282346638528860e+38，6 ~ 7 位有效数字|
|double	|8|	±4.94065645841246544e-324 ~ ±1.79769313486231570e+308，15 ~ 16位有效数字|
|boolean|n/a| `true`, `false`				|

* 默认值都是 0
* 整数字面值默认是 int，带小数的默认是 double

## 类型转换

* 低 → 高：保留原数值（byte < short < int < long < float < double）
* 高 → 低：强制转换，可能带来精度损失
* char 不能直接转换成为 byte 或 short
* boolean 不能直接转换成其他任何类型


## 自动装箱和拆箱（Boxing & Unboxing）

这是 Java 5 的时候加入的特性：基础数据类型和他们对应的类自动转换（编译器来完成这样的工作）

* Number
  * Byte/Short/Integer/Long
  * Float/Double
* Character
* Boolean

```java
Character ch = 'a';

List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2){
	li.add(i);
	// li.add(Integer.valueOf(i));
}

int sum = 0;
for (Integer i: li) {
	sum += i;
	// sum += i.intValue();
}
```

## 浮点数

| 浮点数类型 	| 符号位 	| 阶码 	| 尾码 	|
|---|:-:|:-:|:-:|
| 单精度		| 1 		| 8		| 23 	|
| 双精度		| 1 		| 11 	| 52	|

* 不仅存在最大最小值，同时还存在着最小数字间隔。假如两个数值间隔太小，使用上述浮点数的表示方法，这两个数字的值就是一样的。
* 浮点数运算在不同平台上的实现可能有出入，为了获得统一个结果，可以使用 strictfp
* 注意和整型不同，Float/Double.MIN_VALUE 的值是是它能表示的最小精度，而不是一个负数

## 高精度

* BigInteger（整数）/BigDecimal（实数）
* 加减乘除取余（add，subtract，multiply，divide，mod）当然不止这些计算方法，还有绝对值，最大公约数，位运算，素数测试等

```java
BigInteger i = new BigInteger("123")
```



## Integer 缓存

```java
Integer a=1;
Integer b=1;
Integer c=200;
Integer d=200;
System.out.println(a==b);//true
System.out.println(c==d);//false
```

原因是 Integer 对 -128 到 127 做了缓存，源代码如下：

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```
