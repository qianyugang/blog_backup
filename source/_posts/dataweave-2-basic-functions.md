---
title: DataWeave 2.0 基本函数
toc: true
comments: true
categories: 与代码
tags: 
	- DataWeave
	- iPaaS

date: 2020-07-16
---

本文章中包含用于数据转换的核心 DataWeave 函数。任何DataWeave脚本都可以直接使用。
有关DataWeave 1.0函数的文档，请参阅[DataWeave操作符](https://docs.mulesoft.com/mule-runtime/3.9/dataweave-operators)
**原文地址：**https://docs.mulesoft.com/mule-runtime/4.3/dw-core

## 基本函数

| 函数名 | 描述 |
| ----- | ---- |
| [`++`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-plusplus) | 连接两个值。 |
| [`--`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-minusminus) | 从输入值中移除指定的值。|
| [`abs`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-abs) | 返回一个数的绝对值。 |
| [`avg`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-avg) | 返回数组中列出的数字的平均值。 |
| [`ceil`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-ceil)  | 将一个数四舍五入到最接近的整数。 |
| [`contains`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-contains)  | 如果输入包含给定值，则返回`true`;如果不包含给定值，则返回`false`。 |
| [`daysBetween`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-daysbetween)  | 返回两个日期之间的天数。 |
| [`distinctBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-distinctby)  | 对数组进行去重并返回其中唯一的元素。|
| [`endsWith`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-endswith)  | 如果字符串以所提供的子字符串结尾，则返回`true`，否则返回`false`。  |
| [`entriesOf`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-entriesof)  | 从指定的键、值、对象或任何属性返回一个键值对的数组。 |
| [`filter`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-filter) | 遍历数组并应用返回匹配值的表达式。 |
| [`filterObject`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-filterobject) | 在对象中迭代键-值对，并应用只返回匹配对象的表达式，从输出中过滤掉其余的对象。|
| [`find`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-find) | 返回与指定值匹配的输入的索引。 |
| [`flatMap`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-flatmap) | 遍历数组中的每个项并扁平化结果。 |
| [`flatten`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-flatten) | 将一组子数组(如`[[1,2,3]，[4,5，[6]]，[]，[null]]`)转换为单个扁平数组(如`[1,2,3,4,5，[6]，null]`)。|
| [`floor`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-floor) | 将一个数四舍五入到最接近的整数。 |
| [`groupBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-groupby) | 返回一个对象，该对象根据指定的条件对数组中的项进行分组，例如表达式或匹配的选择器。 |
| [`isBlank`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-isblank) | 如果给定字符串为空或完全由空白组成，则返回`true`，否则返回`false`。 |
| [`isDecimal`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-isdecimal) | 如果给定数字包含小数，则返回`true`，否则返回`false`。 |
| [`isEmpty`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-isempty) |如果给定输入值为空，则返回`true`，否则返回`false`。 |
| [`isEven`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-iseven) | 如果数字或数学操作的数字结果是偶数，则返回`true`，否则返回`false`。 |
| [`isInteger`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-isinteger) | 如果给定的数字是整数(缺少小数)，则返回`true`，否则返回`false`。 |
| [`isLeapYear`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-isleapyear) | 如果给定的值是闰年的日期，返回`true`，否则返回`false`。 |
| [`isOdd`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-isodd) | 如果数字或数学操作的数字结果是奇数，则返回`true`，否则返回`false`。  |
| [`joinBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-joinby) | 将数组合并为单个字符串值，并使用所提供的字符串作为列表中每个项之间的分隔符。 |
| [`keysOf`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-keysof) | 从传入对象中的键-值对返回键数组。 |
| [`log`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-log) | 在不更改输入值的情况下，log将输入作为系统日志返回。 |
| [`lower`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-lower) | 转换为小写字符。 |
| [`map`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-map) | 遍历数组中的项并将结果输出到新数组中。 |
| [`mapObject`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-mapobject) | 遍历一个对象使用一个映射器作用于键,值,或索引的对象。 |
| [`match`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-match) |使用Java正则表达式(regex)匹配字符串，然后将其分割为捕获组。返回数组中的结果。 |
| [`matches`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-matches) | 检查表达式是否与整个输入字符串匹配。 |
| [`max`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-max) |返回数组中`Comparable`的最大值。  |
| [`maxBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-maxby) | 遍历数组并从中返回`Comparable`元素的最大值。 |
| [`min`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-min) | 返回数组中`Comparable`的最小值。 |
| [`minBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-minby) | 遍历数组并从中返回`Comparable`元素的最小值。 |
| [`mod`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-mod) | 取模 |
| [`namesOf`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-namesof) | 返回字符串数组，包含给定对象中所有键的名称。|
| [`native`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-native) | 内部方法，用于指示函数实现不是用DataWeave编写的，而是用Scala编写的。 |
| [`now`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-now) | 返回当前`DateTime` 日期时间。 |
| [`orderBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-orderby) | 使用对输入的选定元素起作用的标准重新排序输入的元素。 |
| [`pluck`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-pluck) |用于将对象映射到数组中，获取对象上的迭代并从该对象返回键、值或索引数组。 |
| [`pow`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-pow) | 幂计算 |
| [`random`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-random) | 返回一个大于或等于0.0且小于1.0的伪随机数。|
| [`randomInt`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-randomint) | 返回从0到指定数字的伪随机整数(专用)。 |
| [`read`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-read) | 读取字符串或二进制文件并返回解析后的内容。 |
| [`readUrl`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-readurl) | 类似于`read`函数。但是，`readURL`接受一个`URL`，包括一个基于类路径的`URL`。 |
| [`reduce`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-reduce) | 对数组中的元素应用表达式。 |
| [`replace`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-replace) | 字符串替换。 |
| [`round`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-round) | 将一个数四舍五入到最接近的整数。 |
| [`scan`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-scan) | 返回一个包含在输入字符串中找到的所有匹配项的数组。|
| [`sizeOf`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-sizeof) | 返回数组中元素的个数。如果数组为空则返回0。 |
| [`splitBy`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-splitby) | 根据与该字符串部分匹配的值将该字符串分割为字符串数组。它从返回的数组中过滤出匹配的部分。 |
| [`sqrt`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-sqrt) | 返回一个数的平方根|
| [`startsWith`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-startswith) | 根据输入字符串是否以匹配的前缀开头，返回`true`或`false`。 |
| [`sum`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-sum) | 返回数组中数值的总和。 |
| [`to`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-to) | 返回具有指定边界的范围。 |
| [`trim`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-trim) | 删除字符串开头和结尾的所有空格。 |
| [`typeOf`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-typeof) | 返回一个值的类型 |
| [`unzip`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-unzip) | 执行与zip相反的操作。它接受数组的数组作为输入。 |
| [`upper`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-upper) | 转换为大写字符串 |
| [`uuid`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-uuid) | 使用随机数作为源返回v4 UUID。 |
| [`valuesOf`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-valuesof) | 返回对象中键-值对中值的数组。 |
| [`with`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-with) | 当与`replace`一起使用时，`with`传递一个替换字符串。 |
| [`write`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-write) | 以支持的格式将值写入字符串或二进制文件。 |
| [`xsiType`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-xsitype) | 创建一个xsi:type类型属性。此方法返回一个对象，因此必须与动态属性一起使用。 |
| [`zip`](https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-zip) | 将两个数组中的元素合并为数组。|


