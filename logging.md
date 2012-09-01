# 错误和记录

## 目录

- [基本设置](#basic-configuration)
- [记录](#logging)
- [Logger类](#the-logger-class)

<a name="basic-configuration"></a>
## 基本设置

所有与错误和记录有关的设置都保存在**application/config/errors.php**文件中，让我们来看看它的内容。

### Ignored Errors

The **ignore** option contains an array of error levels that should be ignored by Laravel. By "ignored", we mean that we won't stop execution of the script on these errors. However, they will be logged when logging is enabled.

### Error Detail

The **detail** option indicates if the framework should display the error message and stack trace when an error occurs. For development, you will want this to be **true**. However, in a production environment, set this to **false**. When disabled, the view located in **application/views/error/500.php** will be displayed, which contains a generic error message.

<a name="logging"></a>
## Logging

To enable logging, set the **log** option in the error configuration to "true". When enabled, the Closure defined by the **logger** configuration item will be executed when an error occurs. This gives you total flexibility in how the error should be logged. You can even e-mail the errors to your development team!

By default, logs are stored in the **storage/logs** direcetory, and a new log file is created for each day. This keeps your log files from getting crowded with too many messages.

<a name="the-logger-class"></a>
## The Logger Class

Sometimes you may wish to use Laravel's **Log** class for debugging, or just to log informational messages. Here's how to use it:

#### Writing a message to the logs:

	Log::write('info', 'This is just an informational message!');

#### Using magic methods to specify the log message type:

	Log::info('This is just an informational message!');
