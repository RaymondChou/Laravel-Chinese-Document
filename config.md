# 运行配置

## 目录

- [基础](#the-basics)
- [查询选项](#retrieving-options)
- [设置选项](#setting-options)

<a name="the-basics"></a>
## 基础

有时候你需要在运行时获取和设置配置选项，**Config**类可以满足你的需要。在Laravel框架**Config**类可以用'.'号来访问配置文件。

<a name="retrieving-options"></a>
##  查询选项

#### 获取一个配置选项:

	$value = Config::get('application.url');

#### 获取选项失败后返回默认值:

	$value = Config::get('application.timezone', 'UTC');

#### 获取配置文件的全部选项:

	$options = Config::get('database');

<a name="setting-options"></a>
## 设置选项

#### 设置一个配置选项:

	Config::set('cache.driver', 'apc');
