# RabbitMQ Windows安装和配置 #
## 下载地址 ##
> **官网windows下载地址:**
> 
> http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.10/rabbitmq-server-3.6.10.exe
> 
> **github windows版下载地址：**
> 
> https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_10/rabbitmq-server-3.6.10.exe
> 

##1.卸载以前的版本##
如果您现有的安装程序正在计划将Erlang VM从32位版本升级到64位版本，则必须先卸载该代理才能升级VM。安装程序将无法停止或删除与不同架构的Erlang VM一起安装的服务.



## 2.安装服务器 ##

RabbitMQ安装依赖环境：erlang

erlang linux 安装详解:
http://flyingdutchman.iteye.com/blog/1885566

1）首先，下载并运行[Erlang Windows二进制文件](http://www.erlang.org/downloads)，大约需要5分钟。

2）然后运行安装程序，rabbitmq-server-3.6.10.exe。大约需要2分钟，并将RabbitMQ设置为一个服务，并配置默认配置.


##3.运行RabbitMQ服务##

> **自定义RabbitMQ环境变量**
> 
> 该服务将使用其默认设置运行良好。您可能需要[自定义RabbitMQ环境](http://www.rabbitmq.com/configure.html#customise-windows-environment)或编辑[配置](http://www.rabbitmq.com/configure.html#configuration-file)。
> 
> **运行RabbitMQ**
> 
> RabbitMQ服务自动启动。您可以从开始菜单stop/reinstall/start RabbitMQ服务.
> 
> **管理服务**
> 
> 您可以在开始菜单中找到RabbitMQ目录的链接。这里也是一个到sbin目录启动的命令提示符窗口的链接，在开始菜单中。这是运行各种命令行工具的最方便的方式。
> 
> **端口访问**
> 
> 防火墙和其他安全工具可能会阻止RabbitMQ绑定到端口.当这种情况发生时，RabbitMQ将无法启动。确保可以打开以下端口：
> 
>  4369：[epmd](http://erlang.org/doc/man/epmd.html)，RabbitMQ节点和CLI工具使用的对等发现服务
>  
>  5672,5671：由AMQP 0-9-1和1.0客户端使用，不带TLS和TLS
>  
>  25672：Erlang分发用于节点间和CLI工具通信，并从动态范围分配（默认情况下限制为单个端口，计算为AMQP端口+ 20000）.有关细节，请参阅[网络指南](http://www.rabbitmq.com/networking.html)。
>  
>  15672：[HTTP API](http://www.rabbitmq.com/management.html)客户端和[rabbitmqadmin](http://www.rabbitmq.com/management-cli.html)（仅当启用[ management plugin](http://www.rabbitmq.com/management.html)时）
>  
>  61613，61614：没有和使用TLS的[STOMP clients](https://stomp.github.io/stomp-specification-1.2.html)（只有启用了[STOMP plugin](http://www.rabbitmq.com/stomp.html)）
>  
>  1883,8883 :(没有和带有TLS的[MQTT clients](http://mqtt.org/)，如果启用了[MQTT plugin](http://www.rabbitmq.com/mqtt.html)
>  
>  15674：STOMP-over-WebSockets clients（只有启用了[Web STOMP plugin](http://www.rabbitmq.com/web-stomp.html)）
>  
>  15675：MQTT-over-WebSockets clients（仅当启用了[Web MQTT plugin](http://www.rabbitmq.com/web-mqtt.html)时）\
>  
>  可以将[RabbitMQ配置](http://www.rabbitmq.com/configure.html)为使用不同的端口。
>  
>  **默认用户访问**
>  
>  代理创建具有密码guest的用户guest。未配置的客户端通常将使用这些凭据.默认情况下，这些凭据只能在以localhost连接到代理时使用，因此在连接任何其他机器之前需要采取措施。
>  
>  有关如何创建更多用户，删除guest用户或允许远程访问guest用户的信息，请参阅[访问控制文档](http://www.rabbitmq.com/access-control.html)
>  
>  **Managing the Broker**
>  
>  要停止代理或检查其状态，请在sbin（作为管理员）中使用rabbitmqctl.bat
>  
>  Stopping the Broker:rabbitmqctl stop.
>  
>  Checking the Broker Status:rabbitmqctl status
>  
>  如果没有代理程序正在运行（即，nodedown），所有的rabbitmqctl命令将报告节点缺失。更多信息参见:[rabbitmqctl](http://www.rabbitmq.com/man/rabbitmqctl.1.man.html)


##4.日志记录##


来自服务器的输出将发送到RABBITMQ\_LOG\_BASE目录中的RABBITMQ_NODENAME.log文件。

其他日志数据写入RABBITMQ_NODENAME-sasl.log。broker总是追加到日志文件，所以保留完整的日志记录.

您可以使用rabbitmqctl rotate_logs轮换日志


##5.作为服务运行时的故障排除##

如果Erlang VM在RabbitMQ作为服务运行时崩溃,不是将崩溃转储写入当前目录（这对服务没有意义），它将写入RabbitMQ服务器的基本目录中的erl_crash.dump文件（由RABBITMQ_BASE环境变量设置）。默认为％APPDATA％\％RABBITMQ_SERVICENAME％ - 通常为％APPDATA％\ RabbitMQ）。
> **Windows特定的问题**
> 
> 我们的目标是使RabbitMQ成为Windows上的一流公民。然而，有时我们无法控制的情况。请参阅[Windows-specific Issues ](http://www.rabbitmq.com/windows-quirks.html)页面
> 
> **获得帮助**
> 
> 如果您有任何问题或需要帮助，请随时询问[RabbitMQ邮件列表](https://groups.google.com/forum/#!forum/rabbitmq-users)







