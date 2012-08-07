# 控制器

## 目录

- [基础](#the-basics)
- [控制器路由](#controller-routing)
- [插件控制器](#bundle-controllers)
- [行动过滤器](#action-filters)
- [嵌套控制器](#nested-controllers)
- [控制器布局](#controller-layouts)
- [REST风格控制器](#restful-controllers)
- [依赖注入](#dependency-injection)
- [控制器工厂](#controller-factory)

<a name="the-basics"></a>
## 基础

控制器是负责处理用户输入和管理模块、库与视图之间交互的类。通常情况下，控制器会向模块请求数据，然后把数据传递给视图，最后返回给用户。

在程序开发中控制器通常被用来实现应用逻辑。Laravel框架还允许开发者在路由中声明应用逻辑，这部分会在[路由文档](/docs/routing)中详细说明。但是我们鼓励新手仍然从控制器开始。在处理应用逻辑方面，控制器和路由没有什么不同。

控制器类都应该存放在**application/controllers**目录中，并且都继承于Base_Controller类。Laravel框架默认自带了一个Home_Controller类。

#### 创建一个简单的控制器:

	class Admin_Controller extends Base_Controller
	{

		public function action_index()
		{
			//
		}

	}

**Actions**是允许被访问的控制器方法，它们都应该以"action\_"为前缀，除此以外的其他方法都是禁止访问的。

> **Note:** Base\_Controller类继承于Laravel框架的Controller类。

<a name="controller-routing"></a>
## 控制器路由

需要我们注意到是，在Laragel框架中所有的路由（包括控制器路由）都必须明确定义。

这意味着没有在路由中注册的控制器方法都是不可见的。使用控制器路由注册之后，控制器方法会自动接受访问。控制器路由的注册信息通常定义在**application/routes.php**文件中。

访问[the routing page](/docs/routing#controller-routing)获取更多关于控制器路由的信息。.

<a name="bundle-controllers"></a>
## 插件控制器

插件包是Laravel框架的模块化管理系统。插件包可以非常容易的配置应用的处理请求。这部分我们将在[插件包]文档中作进一步了解。

创建插件包控制器的方法和创建应用控制器一样，只需要在控制器名前加上插件包名作为前缀。比如你的插件包名字叫"admin"，那么插件包控制器可以这样写：

#### 创建插件控制器:

	class Admin_Home_Controller extends Base_Controller
	{

		public function action_index()
		{
			return "Hello Admin!";
		}

	}

但是我们怎么用路由来注册插件控制器呢？其实也非常简单：

#### 在路由中注册插件控制器:

	Route::controller('admin::home');

现在我们就可以在浏览器中访问"admin"插件包的home控制器了。

> **Note:** 在Laravel框架中，我们使用双冒号来表示插件包，插件包更多信息可以阅读[插件包](/docs/bundles)文档。

<a name="action-filters"></a>
## 行动过滤器

行动过滤器可以运行在控制方法之前,也可以在控制器方法之后.在Laravel框架中你不仅可以为控制器分配过滤器，同时还可以决定何种HTTP请求会触发过滤器。

你可以在控制器构造器中为控制器分配前置或者后置过滤器。

#### 给所有请求附加过滤器:

	$this->filter('before', 'auth');

在这个例子中，'auth'过滤器会运行在所有控制器方法之前。我们可以在**application/routes.php**文件中找到'auth'过滤器。它是用来验证用户是否登录，如果没有就会重定向到'login'。

#### 给少数控制器附加过滤器:

	$this->filter('before', 'auth')->only(array('index', 'list'));

在这个例子中auth过滤器会在action_index()和action_list()方法前运行，用户必须登录才能访问这些页面。但是该控制器中的其他方法不会触发身份验证。

#### 给多数控制器附加过滤器:

	$this->filter('before', 'auth')->except(array('add', 'posts'));

在前一个例子当中，过滤器只会运行在指定的控制器方法之前。而在这个例子中，我们声明的是不需要过滤的控制器方法。

#### 为POST请求附加过滤器:

	$this->filter('before', 'csrf')->on('post');

在这个例子中我给POST请求附加了一个csrf过滤器。'csrf'过滤器主要是用来过滤来自其他系统的posts（比如传说中的spam机器人）。Larvel框架自带了这个过滤器，你可以在**application/routes.php**文件中找到它。

*进阶阅读:*

- *[路由过滤器](/docs/routing#filters)*

<a name="nested-controllers"></a>
## 嵌套控制器

控制器可以存放在**application/controllers**目录的任意层次的子目录中。

创建一个控制器**controllers/admin/panel.php**，代码如下：

	class Admin_Panel_Controller extends Base_Controller
	{

		public function action_index()
		{
			//
		}

	}

#### 在路由中用'.'号来注册嵌套控制器：

	Route::controller('admin.panel');

> **Note:** 当使用嵌套控制器的时候，控制器的注册顺序总是按目录的层次从深到浅。

#### 访问控制器的'index'方法:

	http://localhost/admin/panel

<a name="controller-layouts"></a>
## 控制器布局

控制器布局的完整文档请阅读[模板文档](http://laravel.com/docs/views/templating).

<a name="restful-controllers"></a>
## REST风格控制器

除了使用"action_"前缀之外，我们还可以使用HTTP请求类型来作为控制器方法的前缀。

#### 为控制添加REST风格属性：

	class Home_Controller extends Base_Controller
	{

		public $restful = true;

	}

#### 建立REST风格的控制器方法:

	class Home_Controller extends Base_Controller
	{

		public $restful = true;

		public function get_index()
		{
			//
		}

		public function post_index()
		{
			//
		}

	}

在我们建立CRUD方法的时候这种风格非常的友好。

<a name="dependency-injection"></a>
## 依赖注入

如果你正在编写可测试的代码，你可能会想在你的控制器构造使用依赖注入。这非常简单，只需要在[IoC容器](/docs/ioc)中注册你的控制器即可。在注册控制器时，需要使用**controller**前缀，因此在**application/start.php**文件中，我们可以像下面这样注册控制器：

	IoC::register('controller: user', function()
	{
		return new User_Controller;
	});

当控制器接收请求时，Laravel框架会自动检测控制器在容器中是否注册，如果已注册，那么将会生成一个控制器实例。

> **Note:** 在使用控制器的依赖注入前，你应该阅读[IoC容器](/docs/ioc)文档。

<a name="controller-factory"></a>
## 控制器工厂

如果你想更好的控制控制器实例，那么你就需要是使用Laravel提供的控制器工厂。

**为控制器实例注册一个事件：**

	Event::listen(Controller::factory, function($controller)
	{
		return new $controller;
	});

这个事件会接收需要实例化的类名，然后返回给你一个控制器实例。
