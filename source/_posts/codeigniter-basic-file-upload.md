---
title: CI框架实现基本的文件上传
tags:
  - CodeIgniter
  - PHP
url: 662.html
id: 662
categories:
  - 我是修电脑的
date: 2013-07-09 16:03:17
---

接着[CI框架实现基本的数据库操作](http://www.102no.com/wordpress/?p=653 "CI框架实现基本的数据库操作")这一篇的说，这些其实初学者都很容易弄懂的，因为数据库就是什么增删添改。 所以这一篇来说说上一篇被注释了的部分：文件上传！ 这一回先说视图层好了： 在上一篇刚才的qa.php文件里面的代码：
```
    <form action="insert" method="post" enctype="multipart/form-data">
         标题：<input type="text" name="question" /><br />
         内容：<input type="text" name="answer" /><br />
         图片：<input type="file" name="image" /><br />
         <input type="submit" value="提交" />
    </form>
```
这个表单一定要记者enctype="multipart/form-data"这个东东，没了它一定无法上传，切记切记； 然后是修改页面的视图代码：
```
	<form action="<?php echo "updata"?>" method="post" enctype="multipart/form-data">
		 标题：<input type="text" name="question" /><br />
		 内容：<input type="text" name="answer" /><br />
		 图片：<input type="file" name="image" /><br />
		 <input type="submit" value="提交" />
		 <input type="text" name="id" value="<?php echo "$id"?>" />
	</form>
```
注意action 然后就是在上一篇的控制器feadmin.php加上文件上传类：
```
	//文件上传
	function fileUp(){
		$config\['upload_path'\] = './upload/img';
		$config\['allowed_types'\] = 'gif|jpg|png';//文件类型
		$config\['max_size'\] = '0';
		$config\['encrypt_name'\] = true;
		$this->load->library('upload',$config);
		if ($this->upload->do_upload('image')) {
			$upload_data = $this->upload->data();
			return $upload\_data\['file\_name'\];
		}
	}
```
就是这个它可以实现文件上传， 在插入的时候代码
```
	//插入
	function insert() {
		$_POST\['image'\] = $this->fileUp();
		$data=array(
				'question'=>$_POST\['question'\],
				'answer'=>$_POST\['answer'\],
				'img'=>$_POST\['image'\],
		);
		$this->fe_model->insert($data);
		redirect('feadmin/look','refresh');
	}
```
注意开头的那句$this->fileUp();它就是用那个文件上传类，搞定你要传过来的图片。 然后就是有修改了：是修改，不是修改查询
```
	//修改
	function updata(){
		$_POST\['image'\] = $this->fileUp();

		$id = $this->input->post('id');
		$this->db->where('id',$id);
		$query= $this->db->get('yanzhengma');
		$row = $query->row_array();
		if($row\['img'\] != "")
		{
			$file_path = 'upload/img/'.$row\['img'\];//获取路径
			unlink($file_path);//删除图片
		}
		$_POST\['image'\] = $this->fileUp();
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
依旧是加上了一个unlink()删掉之前的文件。 然后就是和插入一个道理，只不过MYSQL语句换为UPDATE罢了。 代码什么的，[**戳这里下载**](http://pan.baidu.com/share/link?shareid=2703042371&uk=118500861 "源码下载")，两篇文章可能写的很粗浅，代码也都很简单，但是必须从简单开始，省的一口吃个大胖子出错多。
