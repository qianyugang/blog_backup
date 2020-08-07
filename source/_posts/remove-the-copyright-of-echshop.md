---
title: 如何去除Ecshop的底部版权（Powered by ECShop）
tags:
  - ecshop
url: 66.html
id: 66
categories:
  - 我是修电脑的
date: 2012-05-31 08:12:16
---

ecshop版权做的很~~坑爹，今天删掉了的时候，发现，它会在我的page\_footer.lbi里面跳来跳去，感觉不是很好，然后就搜集了一下方法，贴出来，主要的实现原理是删掉出现版权信息的调用标签，还有就是删掉ecshop伪装的js。 

1、 首先修改模板文件， 以官方默认模板为例， 打开ECSHOP模板文件夹（themes/default/library/）下的 page\_footer.lbi 文件 删除下面这一行  
```
{foreach from=$lang.p_y item=pv}{$pv}{/foreach}{$licensed}<br />
```
  
2、 修改JS文件 打开 /js/common.js 文件，找到下面代码并删除之  
```
onload = function()
{
	var link_arr = document.getElementsByTagName(String.fromCharCode(65));
	var link_str;
				var link_text;
				var regg, cc;
				var rmd, rmd\_s, rmd\_e, link_eorr = 0;
				var e = new Array(97, 98, 99,
												100, 101, 102, 103, 104, 105, 106, 107, 108, 109,
												110, 111, 112, 113, 114, 115, 116, 117, 118, 119,
												120, 121, 122
);

try
{
				for(var i = 0; i < link_arr.length; i++)
				{
								link\_str = link\_arr\[i\].href;
								if (link_str.indexOf(String.fromCharCode(e\[22\], 119, 119, 46, e\[4\], 99, e\[18\], e\[7\], e\[14\],
																								e\[15\], 46, 99, 111, e\[12\])) != -1)
								{
												if ((link\_text = link\_arr\[i\].innerText) == undefined)
												{
																throw "noIE";
												}
												regg = new RegExp(String.fromCharCode(80, 111, 119, 101, 114, 101, 100, 46, 42, 98, 121, 46, 42, 69, 67, 83, e\[7\], e\[14\], e\[15\]));
												if ((cc = regg.exec(link_text)) != null)
												{
																if (link_arr\[i\].offsetHeight == 0)
																{
																				break;
																}
																link_eorr = 1;
																break;
												}
								}
								else
								{
												link\_eorr = link\_eorr ? 0 : link_eorr;
												continue;
								}
				}
} // IE
catch(exc)
{
				for(var i = 0; i < link_arr.length; i++)
				{
								link\_str = link\_arr\[i\].href;
								if (link_str.indexOf(String.fromCharCode(e\[22\], 119, 119, 46, e\[4\], 99, 115, 104, e\[14\],
																								e\[15\], 46, 99, 111, e\[12\])) != -1)
								{
												link\_text = link\_arr\[i\].textContent;
												regg = new RegExp(String.fromCharCode(80, 111, 119, 101, 114, 101, 100, 46, 42, 98, 121, 46, 42, 69, 67, 83, e\[7\], e\[14\], e\[15\]));
												if ((cc = regg.exec(link_text)) != null)
												{
																if (link_arr\[i\].offsetHeight == 0)
																{
																				break;
																}
																link_eorr = 1;
																break;
												}
								}
								else
								{
												link\_eorr = link\_eorr ? 0 : link_eorr;
												continue;
								}
				}
} // FF

try
{
				rmd = Math.random();
				rmd_s = Math.floor(rmd * 10);
				if (link_eorr != 1)
				{
								rmd\_e = i - rmd\_s;
								link\_arr\[rmd\_e\].href = String.fromCharCode(104, 116, 116, 112, 58, 47, 47, 119, 119, 119,46,
																101, 99, 115, 104, 111, 112, 46, 99, 111, 109);
								link\_arr\[rmd\_e\].innerHTML = String.fromCharCode(
																80, 111, 119, 101, 114, 101, 100,38, 110, 98, 115, 112, 59, 98,
																121,38, 110, 98, 115, 112, 59,60, 115, 116, 114, 111, 110, 103,
																62, 60,115, 112, 97, 110, 32, 115, 116, 121,108,101, 61, 34, 99,
																111, 108, 111, 114, 58, 32, 35, 51, 51, 54, 54, 70, 70, 34, 62,
																69, 67, 83, 104, 111, 112, 60, 47, 115, 112, 97, 110, 62,60, 47,
																115, 116, 114, 111, 110, 103, 62);
				}
}
catch(ex)
{
}
}
```
 3、 最后别忘了去后台清除一下缓存
