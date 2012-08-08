# 模型与类库

## 目录

- [模型](#models)
- [类库](#libraries)
- [自动加载](#auto-loading)
- [最佳实践](#best-practices)

<a name="models"></a>
## Models

模型是应用程序的核心部分，应用逻辑（控制器/路由）和视图都是用户与模型进行交互的媒介。模型中最典型的逻辑是[商业逻辑](http://en.wikipedia.org/wiki/Business_logic)。

*下面是模型中常见的功能：*

- 数据库交互
- 文件 I/O
- 服务器交互

例如，可能你正在写一个博客程序，那么你就需要一个"post"模型。如果允许用户评论的话，你还需要一个"comment"模型和"User"模型。

<a name="libraries"></a>
## 类库

类库是框架中能够实现某种功能但是不属于某个具体应用的类的集合。例如PDF生成类可以转换HTML文件，它的功能相对复杂，并且可以独立于应用程序之外，我们就可以把它放到"library"（类库）里。

创建一个库类和创建普通的类一样容易，只需要把它放到libraries文件夹中。举个例子，下面我们将创建一个简单的库类，它可以打印我们传入的信息。我们只需要在libraries目录中建立一个**printer.php**文件，内容如下：

	<?php

	class Printer {

		public static function write($text) {
			echo $text;
		}
	}

现在你就可以在应用程序的任何地方调用Printer::write()方法了。

<a name="auto-loading"></a>
## 自动加载

Laravel框架的自动加载器让我们能够非常容易的使用模型和类库。关于自动加载器可以阅读：Auto-Loading](/docs/loading)。

<a name="best-practices"></a>
## 最佳实践

我们经常听到这样一句话："控制器应该保持简洁！"但是在实践中我们怎么才能做到呢？问题的关键在于我们怎么理解"模型"这个词。多数时候模型只被用来负责和数据库交互，这会导致了控制器过度臃肿。让我们来尝试下不同的方法。

如果我们不用"modles"目录又该怎么做呢？让我们为它取个更容易理解的名字，比如我们的卫星导航网站叫"Trackler"，那我们就在应用目录里创建一个"trackler"文件夹。

现在我们把功能分为三个部分："entities"，"services"和"repositories"。然后在"trackler"目录中建立这三个文件夹：

### Entities

我们把entities作为应用的数据容器，他们负责存储属性。在例子中我们有一个"location"，它具有经度和纬度两个属性：

	<?php namespace Trackler\Entities;
	
	class Location {

		public $latitude;
		public $longitude;

		public function __construct($latitude, $longitude)
		{
			$this->latitude = $latitude;
			$this->longitude = $longitude;
		}

	}

### Services

Services包含了应用程序的*流程*。继续用Trackler举例，应用中有一个表单，用户可以输入自己的GPS坐标。但是我们需要验证用户输入坐标的格式，那么我们就在"services"目录中建立一个"validators"文件夹，同时创建一个验证类：

	<?php namespace Trackler\Services\Validators;

	use Trackler\Entities\Location;

	class Location_Validator {

		public static function validate(Location $location)
		{
			// Validate the location instance...
		}

	}

### Repositories

Repositories是应用的数据访问层，它主要负责查询和储存应用的*entities*。继续举例，我们需要一个坐标库来储存坐标，我们可以用任何方式储存：

	<?php namespace Trackler\Repositories;

	use Trackler\Entities\Location;

	class Location_Repository {

		public function save(Location $location, $user_id)
		{
			// Store the location for the given user ID...
		}

	}
现在我们完成了应用程序三个功能的分离，数据库操作被隔离起来，我们就可以在services和controllers里使用respositories，而不用担心切换数据储存方式给应用程序带来影响了。

*深入阅读:*

- [IoC容器](/docs/ioc)
