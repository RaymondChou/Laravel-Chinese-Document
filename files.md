# 文件操作类

## 目录

- [读取文件](#get)
- [写入文件](#put)
- [上传文件](#upload)
- [文件拓展名](#ext)
- [检测文件类型](#is)
- [获取MIME类型](#mime)
- [复制目录](#cpdir)
- [删除目录](#rmdir)

<a name="get"></a>
## 读取文件

#### 读取指定文件内容:

	$contents = File::get('path/to/file');

<a name="put"></a>
## 写入文件

#### 把内容写入文件：

	File::put('path/to/file', 'file contents');

#### 追加内容:

	File::append('path/to/file', 'appended file content');

<a name="upload"></a>
## 上传文件

#### Moving a $_FILE to a permanent location:

	Input::upload('picture', 'path/to/pictures', 'filename.ext');

> **Note:** You can easily validate file uploads using the [Validator class](/docs/validation).

<a name="ext"></a>
## 文件拓展名

#### 获取拓展名:

	File::extension('picture.png');

<a name="is"></a>
## 检测文件类型

#### 确保是指定类型的文件:

	if (File::is('jpg', 'path/to/file.jpg'))
	{
		//
	}

文件操作类的**is**方法并不是仅仅简单的检查文件拓展名，它会使用PHP的Fileinfo拓展来读取文件内容，以确保取得正确的MIME类型。

> **Note:** 你可以在**application/config/mimes.php**中为**is**方法定义添加任何默认允许的拓展名。
> **Note:** 使用函数前需安装Fileinfo拓展，更多相关信息可以查看：[PHP Fileinfo page](http://php.net/manual/en/book.fileinfo.php)。

<a name="mime"></a>
## 获取MIME类型

#### 通过拓展名获取MIME类型：

	echo File::mime('gif');

> **Note:** This method simply returns the MIME type defined for the extension in the **application/config/mimes.php** file.

<a name="cpdir"></a>
## Copying Directories

#### Recursively copy a directory to a given location:

	File::cpdir($directory, $destination);

<a name="rmdir"></a>
## Removing Directories

#### Recursively delete a directory:

	File::rmdir($directory);
