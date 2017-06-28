#  Rabbit MQ config文件解析 #
1. tcp_listeners:用于监听AMQP连接的端口或主机名/对（不带TLS）,默认端口：5672
2. 2.num_tcp_acceptors	:将接受TCP侦听器连接的Erlang进程数。 默认值：10
3. handshake_timeout:AMQP 0-8 / 0-9 / 0-9-1握手（套接字连接和SSL握手后）的最长时间，以毫秒为单位。 默认值：10000
4. ssl_listeners:如上所述，用于SSL连接。 默认值：[]
5. num_ssl_acceptors:将接受SSL侦听器连接的Erlang进程数。 默认值：1
6. ssl_options:SSL配置。请参阅SSL文档。 默认值：[]
7. ssl_handshake_timeout:SSL握手超时，以毫秒为单位。 默认值：5000
8. vm_memory_high_watermark:触发流量控制的内存阈值。请参见基于内存的流量控制文档。 默认值：0.4
9. vm_memory_high_watermark_paging_ratio:队列开始将消息发送到光盘以释放内存的高水位限制的分数。请参见基于内存的流量控制文档。 默认值：0.5
10. disk_free_limit:RabbitMQ存储数据的分区的磁盘空间限制。当可用磁盘空间低于此限制时，会触发流量控制.该值可以相对于RAM的总量设置（例如{mem_relative，1.0}）,该值也可以设置为整数个字节。或者，在信息单元（例如“50MB”）中。默认情况下，可用磁盘空间必须超过50MB。请参阅磁盘警报文档。默认值：50000000
11. log_levels:控制日志记录的粒度。该值是日志事件类别和日志级别对的列表,该级别可以是“none”之一（不记录任何事件），“Error”（仅记录Error）,“warning”（仅记录errors和warning）,“info”（errors，warnings和informational 性消息被记录）或“debug”（errors，warnings,informational和Debug消息都会被记录）。目前有四类定义。其他，目前未分类的事件始终被记录。
类别是:

	channel -所有与AMQP频道有关的活动.
	
	connection -用于所有与网络连接相关的事件
	
	federation -对于与federation有关的所有事件
	
	mirroring -用于所有与镜像队列相关的事件。
	
	默认值：[{connection，info}]

12.frame_max：与客户端协商的帧的最大允许大小（以字节为单位），设置为0表示“无限制”，但会在一些QPid客户端中触发错误.设置较大的值可以提高吞吐量;设置较小的值可能会提高延迟，默认值: 131072
13.channel_max：与客户协商的最大允许通道数，设置为0表示“无限制”。使用更多的通道会增加代理的内存占用。默认:0

14.channel_operation_timeout:通道运行超时（以毫秒为单位）（内部使用，由于消息传递协议的差异和限制，不会直接暴露给客户端）,默认:15000

15.heartbeat:代表心跳延迟的值.在几秒钟内，服务器在connection.tune帧中发送.如果设置为0，则将禁用心跳.客户可能不遵循服务器建议，有关详细信息，请参阅AMQP参考.在大量连接的情况下，禁用心跳可能会提高性能，但是可能导致在存在关闭非活动连接的网络设备的情况下连接丢失。默认值：60（版本3.5.5之前的580）

16.default_vhost：当RabbitMQ从头创建新数据库时，创建虚拟主机。交换机amq.rabbitmq.log将存在于此虚拟主机中。 默认值：<<“/”>>

17.default_user：RabbitMQ从头创建新数据库时要创建的用户名。 默认值：<<“guest”>>

18.default_pass：默认用户的密码。 默认值：<<“guest”>>

19.default_user_tags：默认用户的标签。 默认：[administrator]

20.default_permissions：创建时分配给默认用户的权限。 默认：                   [<<“.*”>>，<<“.*”>>，<<“.*”>>]

21.loopback_users:仅允许通过环回接口（即本地主机）连接到代理的用户列表.如果您希望允许默认访客用户远程连接，则需要将其更改为[]。 默认值：[<<“guest”>>]

22.cluster_nodes:设置此选项可以使节点在第一次启动时自动发生。元组的第一个元素是节点将尝试聚集到的节点。第二个元素是disc或ram，并确定节点类型。Default: {[], disc}

23.server_properties：关于连接的客户端通知的键值对列表。Default: []

24.collect_statistics：统计收集模式。主要与管理插件相关。
可选项是： 

none （do not emit statistics events） 

coarse （emit per-queue/per-channel/per-connection statistics） 

fine （also emit per-message statistics）

Default: none

25.collect_statistics_interval：统计信息收集间隔（以毫秒为单位）。主要与管理插件相关。 默认值：5000

26.management_db_cache_multiplier：影响管理插件将缓存昂贵的管理查询（如队列列表）的时间量.高速缓存会将上次查询的经过时间乘以此值，并将结果缓存一段时间。 默认值：5

27.auth_mechanisms：SASL认证机制，以提供给客户。 默认值：[ 'PLAIN'， 'AMQPLAIN']

28.auth_backends：使用身份验证和授权后端列表。 其他数据库比rabbit\_auth\_backend\_internal可以通过插件。 默认值：[rabbit\_auth\_backend\_internal]

29.reverse_dns_lookups:设置为true以使RabbitMQ在客户端连接上执行反向DNS查找，并通过rabbitmqctl和管理插件来呈现该信息。 默认值：false

30.delegate_count:用于集群内通信的代表进程数。在具有非常大数量的核心并且也是集群的一部分的机器上，您可能希望增加此值。默认值：16

31.trace_vhosts：用于tracer内部使用。你不应该改变这个。 默认值：[]

32.tcp_listen_options：默认套接字选项。你可能不想改变这个。
Default:
[{backlog,       128},
 {nodelay,       true},
 {linger,        {true,0}},
 {exit_on_close, false}]

33.hipe_compile：设置为true.为了预编译RabbitMQ部分与HiPE，一个即时编译器为Erlang。这将以增加启动时间为代价增加服务器吞吐量。您可能会在启动时花费几分钟的时间来获得20-50％的性能.这些数字高度依赖于工作负载和硬件.HiPE支持可能不会被编译到Erlang安装中。如果没有，则启用此选项将仅显示警告消息，正常情况下将启动启动。例如，Debian / Ubuntu用户需要安装erlang-base-hipe软件包.
在某些平台上，HiPE完全不可用，特别是Windows.HiPE在17.5之前的Erlang / OTP版本中已经出现了问题。对于HiPE，强烈推荐使用最近的Erlang / OTP版本。默认值:false

34.cluster\_partition\_handling:如何处理网络分区。可用模式有:

ignore

pause_minority

{pause\_if\_all_down，[nodes]，ignore | autoheal}其中[nodes]是节点名称列表 （例如：['rabbit @ node1'，'rabbit @ node2']）
 
autoheal 
默认值: ignore

35.cluster_keepalive_interval:节点应该如何频繁地向其他节点发送keepalive消息（以毫秒为单位）。请注意，这与net_ticktime不一样;错过的keepalive消息不会导致节点被忽略。 默认值：10000

36.queue_index_embed_msgs_below:消息大小以字节为单位，消息将直接嵌入在队列索引中。建议您在更改之前阅读维护者调整文档。默认:Default: 4096

37.msg_store_index_module:队列索引实现模块。建议您在更改之前阅读维护者调整文档。 默认值：rabbit\_msg\_store\_ets\_index

38.backing\_queue\_module:队列内容的实现模块。你可能不想改变这个。 默认值：rabbit\_variable\_queue

39.msg\_store\_file\_size\_limit:持久的可调整值。你几乎肯定不应该改变这个。 默认值：16777216

40.mnesia\_table\_loading\_retry_limit:在等待群集中的Mnesia表可用时重试的次数。 默认值：10

41.queue\_index\_max\_journal_entries:持久的可调整值。你几乎肯定不应该改变这个。 默认值：65536

42.queue\_master\_locator:队列主位置策略。可用的策略是:

<<"min-masters">>

<<"client-local">>

<<"random">>

有关详细信息，请参阅队列主位置的文档。 默认值：<<“client-local”>>

43.lazy\_queue\_explicit\_ gc\_run\_operation\_threshold:在内存压力下，可调整的值仅适用于惰性队列.这是触发垃圾回收器和其他内存减少活动的阈值.低价值可以降低性能，而且可以提高性能，但会导致更高的内存消耗。你几乎肯定不应该改变这个。 默认值：1000

44.queue\_explicit\_ gc\_run\_operation_threshold：在内存压力下，仅适用于正常队列的可调参数值,这是触发垃圾收集器和其他内存减少活动的阈值。低价值可以降低性能，而且可以提高性能，但会导致更高的内存消耗。你几乎肯定不应该改变这个。 默认值：1000


此外，许多插件可以在配置文件中具有部分，名称为rabbitmq_plugin。我们的维护插件记录在以下位置

rabbitmq\_management

rabbitmq\_management\_agent

rabbitmq\_web\_dispatch

rabbitmq\_stomp

rabbitmq\_shovel

rabbitmq\_auth\_backend\_ldap

**配置条目加密**

可以在RabbitMQ配置文件中加密敏感配置条目（例如密码，包含URL的凭据）。代理在开始时解密加密的条目.请注意，加密配置条目不会使系统有意义地更安全。然而，它们允许RabbitMQ的部署符合各国的规定，要求在配置文件中不应以纯文本形式显示敏感数据。

加密值必须在Erlang加密元组内：{encrypted，...}。以下是默认用户加密密码的配置

	[
	  {rabbit, [
	  {default_user, <<"guest">>},
	  {default_pass,
	{encrypted,
	 <<"cPAymwqmMnbPXXRVqVzpxJdrS8mHEKuo2V+3vt1u/fymexD9oztQ2G/oJ4PAaSb2c5N/hRJ2aqP/X0VAfx8xOQ==">>
	}
	  },
	  {config_entry_decoder, [
	 {passphrase, <<"mypassphrase">>}
	 ]}
	]}
	].


注意config_entry_decoder密钥与RabbitMQ用于解密加密值的密码。密码短语不需要在配置文件中进行硬编码，它可以在一个单独的文件中。

	[
	  {rabbit, [
	  ...
	  {config_entry_decoder, [
	 {passphrase, {file, "/path/to/passphrase/file"}}
	 ]}
	]}
	].


当使用{passphrase，prompt}启动时，RabbitMQ也可以要求操作者输入密码

使用rabbitmqctl和encode命令加密值

    rabbitmqctl encode '<<"guest">>' mypassphrase
    {encrypted,<<"... long encrypted value...">>}
    rabbitmqctl encode '"amqp://fred:secret@host1.domain/my_vhost"' mypassphrase
    {encrypted,<<"... long encrypted value...">>}

如果要解密值，请添加--decode选项

    rabbitmqctl encode --decode '{encrypted, <<"...">>}' mypassphrase
    <<"guest">>
    rabbitmqctl encode --decode '{encrypted, <<"...">>}' mypassphrase
    "amqp://fred:secret@host1.domain/my_vhost"

可以对不同类型的值进行编码。上面的例子编码了二进制文件（<<“guest”>>）和字符串（“amqp：// fred：secret@host1.domain/my_vhost”）。


加密机制使用PBKDF2从密码短语中产生派生密钥。默认散列函数为SHA512，默认迭代次数为1000.默认密码为AES 256 CBC

您可以在配置文件中更改这些默认值：

    [
      {rabbit, [
      ...
      {config_entry_decoder, [
     {passphrase, "mypassphrase"},
     {cipher, blowfish_cfb64},
     {hash, sha256},
     {iterations, 10000}
     ]}
    ]}
    ].    

在命令行：

    rabbitmqctl encode --cipher blowfish_cfb64 --hash sha256 --iterations 10000 \
     '<<"guest">>' mypassphrase 