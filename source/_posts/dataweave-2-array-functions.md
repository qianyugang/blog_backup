---
title: DataWeave 2.0 数组函数
toc: true
comments: true
categories: 与代码
tags: 
	- DataWeave
	- iPaaS

date: 2020-07-21
---

本文章中包含包含处理数组的辅助函数。
要使用这个模块，您必须先将它导入到您的 DataWeave 代码中。
例如，通过添加 `import * from dw::core::Arrays` 到DataWeave 脚本的头部。
**原文地址：**https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays

# 数组函数

| 函数名 | 描述 |
| ----- | ---- |
| [countBy](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-countby) | 计算数组中的元素匹配函数的结果。 |  
| [divideBy](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-divideby) | 将数组分解为子数组，子数组包含指定数量的元素。 |  
| [drop](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-drop) | 去掉前`n`个元素。当`n <= 0`时返回原始数组，当`n > sizeOf(array)`时返回空数组。 |  
| [dropWhile](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-dropwhile) | 当满足条件时，删除数组中的元素，但当到达不满足条件的元素时，停止选择过程。 |  
| [every](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-every) | 如果数组中的每个元素都与条件匹配，则返回`true`。 |  
| [firstWith](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-firstwith) | 返回满足条件的第一个元素，如果没有元素满足条件，则返回`null`。 |  
| [indexOf](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-indexof) | 返回数组中某个元素的第一个匹配项的索引。 |  
| [indexWhere](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-indexwhere) | 返回与数组中条件匹配的元素的第一个匹配项的索引。 |  
| [join](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-join) | 根据给定的ID条件连接两个对象数组。 |  
| [leftJoin](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-leftjoin) | 根据给定的ID条件连接两个对象数组。 |  
| [outerJoin](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-outerjoin) | 根据给定的ID条件连接两个对象数组。 |  
| [partition](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-partition) | 将数组分隔为满足条件的元素和不满足条件的元素。 |  
| [slice](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-slice) | 选择满足条件的元素的间隔:`from <= indexOf(array) < until` |  
| [some](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-some) | 如果数组中至少有一个元素与指定条件匹配，则返回true。 |  
| [splitAt](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-splitat) | 在给定位置将数组一分为二。 |  
| [splitWhere](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-splitwhere) | 在满足条件的第一个位置将数组一分为二。|  
| [sumBy](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-sumby) | 返回数组中元素值的总和。 |  
| [take](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-take) | 选择前`n`个元素。当`n <= 0`时返回一个空数组，当`n > sizeOf(array)`时返回原始数组。 |  
| [takeWhile](https://docs.mulesoft.com/mule-runtime/4.3/dw-arrays-functions-takewhile) | 当满足条件时从数组中选择元素，但当到达不满足条件的元素时停止选择过程。 |  



