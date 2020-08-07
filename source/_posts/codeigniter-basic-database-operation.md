---
title: CI框架实现基本的数据库操作
tags:
  - CodeIgniter
  - PHP
url: 653.html
id: 653
categories:
  - 我是修电脑的
date: 2013-07-09 15:23:30
---

凡是学什么东西就要从头学起，哪怕是一个小小的功能也必须要写出来，不要怕别人看见，即使写的很臃肿，或者是很难看，也没关系，这些都是你成长必须的，这些日子重新捡起来CI框架了，这个是比较适合初学者学习的框架，因为它的分层非常的明确，我的一个好基友用的是Yii，他对CI也非常熟悉，他说Yii基本都可以自己生成一些东西，没必要那么麻烦；其实吧，框架乃至于任何代码都是一种工具，至于如何使用，就看你自己的技巧人品和造化。 其实关于模型和控制器，看你如何写，如何用，你也可以全部都在模型里面把数据全部读取，也可以只在其中一个，我们主管说，反正三层呢，能少用一层是一层，于是乎： 控制器来了，在controllers文件夹下新建feadmin.php：
```
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');
```
这个东东是必须的，如果没有的话，会出现一些莫名其妙的问题；然后接下来：
```
class Feadmin extends CI_Controller{
	function __construct(){
		parent::__construct();
		$this->load->database();
		$this->load->model('fe_model');
		$this->load->helper('url');
	}
```
身为一个初学者的话一定要多看手册，关于类名和文件名必须相同什么的这样的超级低级错误，一定要小心；然后就是查看：
```
//查看
	public function look(){
		$query = $this->fe_model->get();
		$get\['qalist'\] = $query->result_array();
		$this->load->view('qa',$get);
	}
```
手册里面说了不推荐驼峰命名法，于是下划线就变成一个很好用的东西；接下来插入：
```
	//插入
	function insert() {
		//$_POST\['image'\] = $this->fileUp();
		echo $_POST\['question'\];
		echo $_POST\['image'\];
		$data=array(
				'question'=>$_POST\['question'\],
				'answer'=>$_POST\['answer'\],
				'img'=>$_POST\['image'\],
		);
		$this->fe_model->insert($data);
		redirect('feadmin/look','refresh');
	}
```
里面有一个FileUP()函数，是一个文件上传类，所以这句话可以不要先,然后是删除：
```
//删除
	function delete(){
		$id = $this->input->get('id');
		$this->db->where('id',$id);
		$query= $this->db->get('yanzhengma');
		$row = $query->row_array();
		//echo $row\['img'\];
		//if($row\['img'\] != "")
		//{
		//	$file_path = 'upload/img/'.$row\['img'\];//获取路径
		//	unlink($file_path);//删除图片
		//}
		$this->db->where('id',$id);
		$this->db->delete('yanzhengma');
		redirect('feadmin/look','refresh');
	}
```
上面我注释掉的那几行代码呢，在下一篇文章会说到，关于简单的文件上传什么的。接下来是查询修改，注意是查询修改哦，不是修改，就是只是把你要修改的数据查询出来
```
//查询修改
	function select_updata(){
		$sel\['id'\] = $this->input->get('id');
		$this->load->view('qa_up',$sel);
	}
```
这里才是真正的修改：
```
	//修改
	function updata(){
		$_POST\['image'\] = $this->fileUp();
		$id = $this->input->post('id');
		$this->db->where('id',$id);
		$query= $this->db->get('yanzhengma');
		$row = $query->row_array();
		//if($row\['img'\] != "")
		//{
		//	$file_path = 'upload/img/'.$row\['img'\];//获取路径
		//	unlink($file_path);//删除图片
		//}
		//echo $_POST\['image'\];
		$_POST\['image'\] = $this->fileUp();
		//echo $_POST\['image'\];
		$sel = array(
				'question'=>$_POST\['question'\],
				'answer'=>$_POST\['answer'\],
				'img'=>$_POST\['image'\],
		);
		if(!empty($id) && (count($sel)>1)){///判断id值是否传过来并且判断封装的数组是否有元素存在
			$this->db->where('id',$id);
			$this->db->update('yanzhengma',$sel);
		};
		redirect('feadmin/look','refresh');
	}
```
依旧照例的呢，上面注释的那部分内容是用来删除图片什么的，如果木有图片什么的，可以无视掉。 是不是要到模型了？没关系，这个暂时没有用到模型，其实把上面的代码里面的DB的部分换到模型，其实一样可以用。 终于到视图层了：在view文件夹下新建qa.php:
```
<body>
	<table > 
	<?php foreach ($qalist as $i): ?>
		<tr  style="text-align:left;" >  
			<td><?php echo $i\['id'\] ;?></td> 
			<td><?php echo $i\['img'\] ;?></td> 
			<td><?php echo $i\['question'\] ;?></td> 
			<td><?php echo $i\['answer'\] ;?></td>
			<td>
				<a href="<?php echo "select_updata" ?>?id=<?php echo $i\['id'\]?>">修改</a> |
                <a href="<?php echo "delete" ?>?id=<?php echo $i\['id'\]?>">删除</a>
			</td> 
	<?php endforeach; ?>

		<table>

	<form action="insert" method="post" enctype="multipart/form-data">
		 标题：<input type="text" name="question" /><br />
		 内容：<input type="text" name="answer" /><br />
		 图片：<input type="file" name="image" /><br />
		 <input type="submit" value="提交" />
	</form>
</body>
```
恩，没错，添加的表单和查看的，都在这里。 然后是修改的视图，在view文件夹下新建qa_up.php
```
<body>
	<form action="<?php echo "updata"?>" method="post" enctype="multipart/form-data">
		 标题：<input type="text" name="question" /><br />
		 内容：<input type="text" name="answer" /><br />
		 图片：<input type="file" name="image" /><br />
		 <input type="submit" value="提交" />
		 <input type="text" name="id" value="<?php echo "$id"?>" />
	</form>
</body>
```
然后就可以输入`http://localhost/test/index.php/feadmin/look`自己去闹一闹了。 待会下一篇把文件上传的东东和代码补上去。
