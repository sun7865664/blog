---
title: java object equals方法
language: zh-CN
date: 2021-05-18 16:19:57
tags: 
- java基础
- object
- equals
---
#### java object equals方法
##### equals
   > Object类中的equals方法用于检测一个对象是否等于另外一个对象
#### java语言规范要求equals方法具有以下特性
 - 1、<b>自反性</b>：对于任何非空引用x，x.equals(x)均应该返回true。
 - 2、<b>对称性</b>：对于任何引用x和y，当且仅当y.equals(x)返回true时，x.equals(y)也应该返回true。
 - 3、<b>传递性</b>：对于任何引用x、y和z，如果x.equals(y)返回true，y.equals(z)返回true，x.equals(z)也应该返回true;
 - 4、<b>一致性</b>：如果x和y的引用对象没有发生变化。反复调用x.equals(y)应该返回同样的结果。
 - 5、对于任意非空引用x，x.equals(null)应该返回false。

