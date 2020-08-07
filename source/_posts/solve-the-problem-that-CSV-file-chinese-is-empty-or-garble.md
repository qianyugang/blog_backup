---
title: 简单的PHP导入csv文件并解决中文为空 / 中文乱码问题
tags:
  - PHP
url: 928.html
id: 928
categories:
  - 我是修电脑的
date: 2014-12-18 19:17:00
---

工作中有时候会遇到会把一些excel表格插入到数据库（这里是mysql）中的场景，这里有比较简单的解决方案，就是先把这个表格另存为csv格式，然后用PHP程序中的fgetcsv函数来读取。 代码如下：
```
function getcsv(){
    $file = fopen("1.csv","r");//文件位置
    while(! feof($file)){
        $row = _fgetcsv($file);
        echo $row\['0'\]." | ".$row\['1'\]." | ".$row\['2'\] ." | ".$row\['3'\] ."\\r\\n";//每个下标对应一列
    }
    fclose($file);
}
```
然后呢，就会发生一个奇怪的问题：文件中的中文要么变成了乱码，要么压根就读不出来，经过查阅，这里算是这个函数的一个小小的bug，那么，此时我们只需要重写一下这个函数就好了，如下：
```
 function _fgetcsv(&$handle, $length = null, $d = ',', $e = '"') {
         $d = preg_quote($d);
         $e = preg_quote($e);
         $_line = "";
         $eof=false;
         while ($eof != true) {
             $_line .= (empty ($length) ? fgets($handle) : fgets($handle, $length));
             $itemcnt = preg\_match\_all('/' . $e . '/', $_line, $dummy);
             if ($itemcnt % 2 == 0)
                 $eof = true;
         }
         $\_csv\_line = preg\_replace('/(?: |\[ \])?$/', $d, trim($\_line));
         $\_csv\_pattern = '/(' . $e . '\[^' . $e . '\]*(?:' . $e . $e . '\[^' . $e . '\]*)*' . $e . '|\[^' . $d . '\]*)' . $d . '/';
         preg\_match\_all($\_csv\_pattern, $\_csv\_line, $\_csv\_matches);
         $\_csv\_data = $\_csv\_matches\[1\];
         for ($\_csv\_i = 0; $\_csv\_i < count($\_csv\_data); $\_csv\_i++) {
             $\_csv\_data\[$\_csv\_i\] = preg\_replace('/^' . $e . '(.*)' . $e . '$/s', '$1' , $\_csv\_data\[$\_csv_i\]);
             $\_csv\_data\[$\_csv\_i\] = str\_replace($e . $e, $e, $\_csv\_data\[$\_csv_i\]);
         }
         return empty ($\_line) ? false : $\_csv_data;
    }
```
然后把getcsv这个函数中的fgetcsv替换为自定义的_fgetcsv就可以了。 参考内容：[http://php.net/manual/zh/function.fgetcsv.php](http://php.net/manual/zh/function.fgetcsv.php)
