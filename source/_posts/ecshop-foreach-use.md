---
title: Ecshop模板中foreach的使用方法
tags:
  - ecshop
url: 78.html
id: 78
categories:
  - 我是修电脑的
date: 2012-05-31 08:37:29
---

1:foreach使用规则，他有以下几个参数 from ,item name iteration index 2:如何使用foreach循环 如果php要传递一个数组（如：$array）给ecshop的smarty模板．那么我们将通过from=$array 来接受，写法是
```
{foreach from = $array item = item}
```
3: ecshop中smarty的下标如何表示，请看下面的例子：
```
{foreach from = $array item = item name=name}
	{$smarty.foreach.name.iteration}
{/foreach}
```
这里的iteration就是从1开始的下标， 如果要从０开始的下标，应该使用{$smarty.foreach.name.index} 4:如何判断是否是foreach循环的开始和结束，最后一个元素．
```
{if $smarty.foreach.last}
```
表示循环的最后一个元素．
```
{if $smarty.freach.first}
```
表示循环的开始．   5:如何使用双重循环． 举例如下：
```
{foreach from = $test item =item}

	{foreach from=$item.children item=child}
		{$child.name}
	{/foreach}
{/foreach}
```
