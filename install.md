# 安装与设置

## 目录

- [要求](#requirements)
- [安装](#installation)
- [服务器配置](#server-configuration)
- [基本设置](#basic-configuration)
- [环境设置](#environments)
- [友好的链接](#cleaner-urls)

<a name="requirements"></a>
## 安装要求

- Apache, nginx, 或者其他web服务器。
- Laravel 框架应用了很多PHP 5.3版才具备的强大的新特性，所以你必须安装PHP5.3或者以上版本。
- Laravel 使用[FileInfo库](http://php.net/manual/en/book.fileinfo.php)来检测mime类型。PHP 5.3版已经默认包含了FileInfo库。Windows用户需要在php.ini中启用该模块。关于FileInfo库的更多信息请阅读：[installation / configuration details on PHP.net](http://php.net/manual/en/fileinfo.installation.php)。
- Laravel 使用[Mcrypt库](http://php.net/manual/en/book.mcrypt.php) 来加密和生成哈希。PHP 5.3已经预装了Mcrypt库。如果你在phpinfo()中没有找到Mcrypt已经启用的信息，请检查你的服务器环境是否安装完全，或者查看PHP手册中 [Mcrypt库](http://php.net/manual/en/book.mcrypt.php)有关信息。

<a name="installation"></a>
## 安装

1. [下载 Laravel](http://laravel.com/download)
2. 解压Laravel压缩包，然后上传文件到你的web服务器。
3. 在config/application.php中设置application key，你可以设置为任意的32位字符串。
4. 确保`storage/views`目录具有写入权限。
5. 现在你可以尝试在浏览器中运行框架。

如果不出意外，你应该看到了Laravel漂亮的初始页面。一切准备就绪，我们可以继续Laravel学习之旅!

### 选装程序

如果你想充分了解和学习Laravel框架的应用，推荐你安装以下程序：

- SQLite, MySQL, PostgreSQL, 或者 SQL Server PDO driver.
- Memcached 或者 APC.

### 安装遇到问题？

如果你在安装过程中遇到了问题，可以检查以下情况：

- 请确保**public**文件夹是服务器的根目录，如果不是，你可以尝试访问Laravel的public文件夹，如http:localhost/public/。
- 如果你启用了mod_rewrite拓展，请把**application/config/application.php**文件中的**index**参数设置为空。
- 请确保服务器的storage目录及其子目录具有写入权限。

<a name="server-configuration"></a>
## 服务器配置

Laravel框架只把允许公众访问的文件放在web服务器的根目录(public)，可以有效的防止因为服务器的设置错误而泄露重要信息资料（译者：前提是你遵守Laravel的安装步骤，如果你的整个Laravel框架在服务器的根目录，请检查除public以外其他目录的访问权限）。

<a name="basic-configuration"></a>
## 基本设置

Laravel框架的所有配置文件都存放在应用程序的config文件夹中，建议你把所有的配置文件都看一下，对应用程序的设置可以有一个基本的了解。你应该注意一下**application/config/application.php**文件，它包含了应用程序的基本设置。

在开发或者启用网站之前，你应该首先修改**application/config/application.php**中的**application key**。Laravel会用它来加密或者生成哈希。你可以手动设置一个32位的随机字符串，也可以使用Laravel提供的Artisan命令行工具来生成一个符合标准的字符串。有关Aartisan工具信息可以访问：Artisan command index](/docs/artisan/commands)。

> **Note:** 如果你启用了mod_rewrite，你应该把index参数设置为空。

<a name="environments"></a>
## 环境设置

通常情况下，应用程序的开发环境和正式生产环境的设置是不同的，Laravel使用的URL机制可以让你轻而易举的解决这个问题。打开Laravel框架的安装目录下的**paths.php**文件，你可以看到下面的数组：

	$environments = array(

		'local' => array('http://localhost*', '*.dev'),

	);

这个数组表示任何以"localhost"开头或者以"*.dev"结尾的请求，都被视为"local"环境。

然后，建立**application/config/local**文件夹，那么local文件夹下面的任何设置都会覆盖**application/config**中的基本设置。举个例子，你希望在新建的**local**目录中创建一个**application.php**文件：

	return array(

		'url' => 'http://localhost/laravel/public',

	);

在这个例子中，local中的**URL**设置会覆盖**application/config/application.php**文件中的**URL**设置。需要注意的是，你只需要指定那些你想要覆盖的设置。

Laravel的环境设置就是这么简单，你可以用它来创建你需要的环境。

<a name="cleaner-urls"></a>
## 友好的链接

通常情况下，你不想"index.php"出现在网站的链接中，那么你可以用rewrite重定向来去掉"index.php"。如果你使用的是Apache服务器，请启用mod_rewrite模块，然后在你的public目录建立一个**.htaccess**文件,内容如下：

	<IfModule mod_rewrite.c>
	     RewriteEngine on

	     RewriteCond %{REQUEST_FILENAME} !-f
	     RewriteCond %{REQUEST_FILENAME} !-d

	     RewriteRule ^(.*)$ index.php/$1 [L]
	</IfModule>

如果上面的htaccess文件没有效果，可以试试下面的写法：

	Options +FollowSymLinks
	RewriteEngine on

	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d

	RewriteRule . index.php [L]

在设置了重定向之后，你还应该把**application/config/application.php**中的**index**参数设置为空。

> **Note:** 不同服务器的rewrite方法有所不同，请根据具体情况配置。

## Links

- [目录](https://github.com/zither/Laravel-Chinese-Document/blob/master/README.md)
- 下一节
