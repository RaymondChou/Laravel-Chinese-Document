# 插件包

## 内容

- [基础](#the-basics)
- [创建插件包](#creating-bundles)
- [注册插件包](#registering-bundles)
- [插件包与类加载](#bundles-and-class-loading)
- [启动插件包](#starting-bundles)
- [插件包路由](#routing-to-bundles)
- [使用插件包](#using-bundles)
- [插件包的静态文件](#bundle-assets)
- [安装插件包](#installing-bundles)
- [升级插件包](#upgrading-bundles)

<a name="the-basics"></a>
## 基础

插件包是Laravel 3.0 改进中最核心的部分。插件包让我们能更加方便的把代码组织到一个模块中。每个插件包都可以包含自己的视图、设置、路由、数据库迁移、任务等。你可以把插件包用到任何地方，它可以是一个对象关系数据库映射，也可以是一个强大的身份验证系统。插件包的代码模块化是驱动Laravel所有设计决策的重要方面。你甚至可以把整个application目录看作是一个Laravel框架默认加载和使用的插件包。

<a name="creating-and-registering"></a>
## 创建插件包

在创建一个插件包之前，你必须在**bundles**目录为它新建一个文件夹。比如我们要建立一个"admin"插件包，作为我们应用的管理后台。在**application/start.php**文件中提供的基本配置可以帮助我们限定应用程序如何运行。同样，我们要在新建的插件包文件夹中创建一个start.php文件，每次加载插件包的时候它都会运行。让我们来创建它：

#### 为插件包创建一个start.php文件:

	<?php

	Autoloader::namespaces(array(
		'Admin' => Bundle::path('admin').'models',
	));

在这个start文件中，我们让自动加载类自动加载在admin命名空间中定义的models目录下的类。你可以在start文件中做任何事情，但是通常情况下，我们用它来自动加载需要的文件。**事实上，你并不一定要为应用包建立一个启动文件。**

然后，我们来看怎么为应用程序注册一个插件包！

<a name="registering-bundles"></a>
## 注册插件包

先我们建立了一个admin插件包，我们需要在Laravel中注册它。打开**application/bundles.php**文件。我们应用程序使用的所有插件包都在这里注册的。让我们把admin插件包添加进去：

#### 注册一个简单的插件包:

	return array('admin'),

默认情况下，Laravel会假设Admin插件包是在插件目录的根目录下，但是我们也可以自定义插件包路径。

#### 注册一个自定义路径的插件包:

	return array(

		'admin' => array('location' => 'userscape/admin'),

	);

现在Laravel会在**bundles/userscape/admin**目录中寻找admin插件包。

<a name="bundles-and-class-loading"></a>
## 插件包与类加载

通常情况下，插件包的**start.php**文件只包含了自动加载注册信息。因此有时候你希望跳过**start.php**文件，直接在注册数组中声明自己的插件包映射信息。可以用下面的方法：

#### 在插件注册数组中定义自动加载映射：

	return array(

		'admin' => array(
			'autoloads' => array(
				'map' => array(
					'Admin' => '(:bundle)/admin.php',
				),
				'namespaces' => array(
					'Admin' => '(:bundle)/lib',
				),
				'directories' => array(
					'(:bundle)/models',
				),
			),
		),

	);

请注意，这里的每一项设置都对应Laravel[auto-loader](/docs/loading)中的一个函数。事实上，每一项设置的参数都会自动传递给auto-loader中对应的函数。

你还要注意下**(:bundle)**占位符。它会自动替换为插件包的路径。

<a name="starting-bundles"></a>
## 启动插件包

现在我们已经创建并注册了插件包，但是我们还不能使用它，我们需要先启动它：

#### 启用一个插件包:

	Bundle::start('admin');

这行代码告诉Laravel执行插件包的**start.php**文件，并在auto-loader中注册。如果插件包存在**routes.php**文件，start方法也会自动加载。

> **Note:** 插件包只会启动一次，重复的请求都会被start方法忽略。

如果你在应用程序中使用了一个插件包，但是又不想每次使用前都手动启动它。那么你可以在**application/bundles.php**文件中，把它设置为自动启动：

#### 把插件包设置为自动启动:

	return array(

		'admin' => array('auto' => true),

	);

如果插件包被设置为自动启动，那你不必要再去手动启用一个插件包，你只需要直接使用它，剩余的工作都交给Laravel去完成。如果你尝试使用插件包的视图文件，那么插件包的配置文件、语言文件、路由或者过滤器都会自动启用。

每当一个插件包启动成功，都将触发一个事件。你可以用下面的方法来监听已经启动的插件包：

#### 监听插件包的启动事件:

	Event::listen('laravel.started: admin', function()
	{
		// The "admin" bundle has started...
	});

同样还可以将一个插件包设置为不可启用，那么这个插件包就不能被启用了。

#### 把插件包设置为不可启用:

	Bundle::disable('admin');

<a name="routing-to-bundles"></a>
## 插件包路由

更多关于插件包路由和控制器的信息可以阅读：[bundle routing](/docs/routing#bundle-routes)和[bundle controllers](/docs/controllers#bundle-controllers)。

<a name="using-bundles"></a>
## 使用插件包

前面已经说过，插件包可以包含自己的视图、配置、语言等文件。在Laravel框架中可以通过双冒号来使用它们。让我们看下面的例子：

#### 获取插件包视图:

	return View::make('bundle::view');

#### 获取插件包配置:

	return Config::get('bundle::file.option');

#### 获取插件包语言:

	return Lang::line('bundle::file.line');

有些时候你想要获取插件包更多的"meta"信息，比如插件包是否存在、插件包路径或者插件包的配置数组。那么可以用下面的方法：

#### 确认插件包是否存在:

	Bundle::exists('admin');

#### 获取插件包的安装路径:

	$location = Bundle::path('admin');

#### 获取插件包的配置:

	$config = Bundle::get('admin');

#### 获取所有已安装的插件包名:

	$names = Bundle::names();

<a name="bundle-assets"></a>
## 插件包静态文件

如果你的插件包包含视图，并且希望它们能像Javascript和images文件一样在**public**目录被调用。方法很简单，只需要在你的插件包目录中新建一个**public**文件夹，然后把静态文件放到里面即可。

但是现在我们怎么把它们放到应用程序的**public**文件夹中呢？Laravel框架的"Artisan"命令行工具提供了一个简单的命令可以把插件包assets文件夹中的文件复制到public目录中：

#### Publish bundle assets into the public directory:

	php artisan bundle:publish

这条命令会在public目录中为bundles创建一个**public/bundles**目录。比如你的插件包叫"admin"，那么就会建立一个**public/bundles/admin**文件夹，里面包含了所有admin插件包public目录里的文件。

更多与插件包assets相关的信息可以阅读：[asset management](/docs/views/assets#bundle-assets)。

<a name="installing-bundles"></a>
## 安装插件包

很多时候我们都需要手动安装插件包，不过"Artisan"CLI工具提供了一种更好的方法来安装和升级插件包。Laravel可以通过解压ZIP压缩包来安装插件包：

#### 通过Artisan安装插件包:

	php artisan bundle:install eloquent

命令执行完插件包就安装成功了，现在你可以[注册它](#registering-bundles)，[发布它的assets文件](#bundle-assets)了。

可以访问官方的[Laravel bundle directory](http://bundles.laravel.com)获得优秀的插件包。

<a name="upgrading-bundles"></a>
## 升级插件包

当你升级插件包时，Laravel会自动移除旧的插件包，然后重新安装。

#### 通过Artisan升级插件包:

	php artisan bundle:upgrade eloquent

> **Note:** 在升级插件包之后,你需要重新[发布插件包的assets文件](#bundle-assets)。

**重要提醒:** 当插件包在升级重新安装成功之后，你必须留意插件包升级前的所有变动，你可能需要再次修改插件包的配置，修改插件包的目录等。可以用插件包的启动事件来设置它们，把类似下面的代码加入到**application/start.php**文件中：

#### Listening for a bundle's start event:

	Event::listen('laravel.started: admin', function()
	{
		Config::set('admin::file.option', true);
	});
