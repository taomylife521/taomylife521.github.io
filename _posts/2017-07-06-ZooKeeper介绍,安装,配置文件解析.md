#什么是ZooKeeper?#

ZooKeeper是用于维护配置信息，命名，提供分布式同步和提供组服务的集中式服务。

所有这些类型的服务都以分布式应用程序以某种形式或另一种形式使用。每次实施时，都有很多工作需要修复不可避免的错误和竞争条件。由于实施这些服务的难度很大，应用程序最初通常会吝啬他们，这使得它们在存在变化时变脆，难以管理。即使正确完成，这些服务的不同实现也会导致在部署应用程序时的管理复杂性。

#ZooKeeper下载地址#

[http://mirrors.hust.edu.cn/apache/zookeeper/](http://mirrors.hust.edu.cn/apache/zookeeper/)


#ZooKeeper Windows下安装#
1. 需先安装JDK
2. 修改配置文件；Zookeeper 的配置文件在 conf 目录下，这个目录下有 zoo_sample.cfg 和 log4j.properties，需要将 zoo_sample.cfg 改名为 zoo.cfg，因为 Zookeeper 在启动时会找这个文件作为默认配置文件.
3. 修改：
 `dataDir=绝对路径

dataLogDir=绝对路径`

4.进入bin目录，运行zkServer.cmd脚本，至此服务就跑起来了！

#ZooKeeper配置文件解析#
    
<div class="cnblogs_Highlighter">
<pre class="brush:csharp;gutter:true;">#ZK中的一个时间单元。ZK中所有时间都是以这个时间单元为基础，进行整数倍配置的。例如，session的最小超时时间是2*tickTime。 The number of milliseconds of each tick
tickTime=2000

# The number of ticks that the initial 
#synchronization phase can take
# Follower在启动过程中，会从Leader同步所有最新数据，然后确定自己能够对外服务的起始状态。Leader允许F在 initLimit 时间内完成这个工作。
#通常情况下，我们不用太在意这个参数的设置。如果ZK集群的数据量确实很大了，F在启动的时候，从Leader上同步数据的时间也会相应变长，因此在这种情况下，有必要适当调大这个参数了
initLimit=10

# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
#在运行过程中，Leader负责与ZK集群中所有机器进行通信，例如通过一些心跳检测机制，来检测机器的存活状态。
#如果L发出心跳包在syncLimit之后，还没有从F那里收到响应，那么就认为这个F已经不在线了。注意：不要把这个参数设置得过大，否则可能会掩盖一些问题
syncLimit=5

# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
#存储快照文件snapshot的目录。默认情况下，事务日志也会存储在这里。建议同时配置参数dataLogDir, 事务日志的写性能直接影响zk性能。
dataDir=/tmp/zookeeper

# the port at which the clients will connect
#客户端连接server的端口，即对外服务端口，一般设置为2181吧。
clientPort=2181


# the maximum number of client connections.
# increase this if you need to handle more clients
#单个客户端与单台服务器之间的连接数的限制，是ip级别的，默认是60，如果设置为0，那么表明不作任何限制。
#请注意这个限制的使用范围，仅仅是单台客户端机器与单台ZK服务器之间的连接数限制，不是针对指定客户端IP，也不是ZK集群的连接数限制，也不是单台ZK对所有客户端的连接数限制。
#指定客户端IP的限制策略，这里有一个patch，可以尝试一下：http://rdc.taobao.com/team/jm/archives/1334
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#这个参数和下面的参数搭配使用，这个参数指定了需要保留的文件数目。默认是保留3个。 New in 3.4.0
#autopurge.snapRetainCount=3

# Purge task interval in hours
# Set to "0" to disable auto purge feature
#3.4.0及之后版本，ZK提供了自动清理事务日志和快照文件的功能，这个参数指定了清理频率，单位是小时，需要配置一个1或更大的整数，默认是0，表示不开启自动清理功能
#autopurge.purgeInterval=1
</pre>
</div>
<p>　　</p>

## 其它未在默认配置文件中的配置参数: ##
<table style="height: 176px; width: 545px;" border="0">
<tbody>
<tr>
<td><strong>dataLogDir</strong></td>
<td>事务日志输出目录。尽量给事务日志的输出配置单独的磁盘或是挂载点，这将极大的提升ZK性能。 &nbsp;</td>
</tr>
<tr>
<td><strong>globalOutstandingLimit</strong></td>
<td>最大请求堆积数。默认是1000。ZK运行的时候， 尽管server已经没有空闲来处理更多的客户端请求了，但是还是允许客户端将请求提交到服务器上来，以提高吞吐性能。当然，为了防止Server内存溢出，这个请求堆积数还是需要限制下的。&nbsp;&nbsp;<br />(Java system property:<strong>zookeeper.globalOutstandingLimit.</strong>&nbsp;)</td>
</tr>
<tr>
<td><strong>preAllocSize</strong></td>
<td>预先开辟磁盘空间，用于后续写入事务日志。默认是64M，每个事务日志大小就是64M。如果ZK的快照频率较大的话，建议适当减小这个参数。(Java system property:<strong>zookeeper.preAllocSize</strong>&nbsp;)</td>
</tr>
<tr>
<td><strong>traceFile</strong></td>
<td>用于记录所有请求的log，一般调试过程中可以使用，但是生产环境不建议使用，会严重影响性能。(Java system property:&nbsp;<strong>requestTraceFile</strong>&nbsp;)</td>
</tr>
<tr>
<td><strong>clientPortAddress</strong></td>
<td>对于多网卡的机器，可以为每个IP指定不同的监听端口。默认情况是所有IP都监听&nbsp;<strong>clientPort</strong>&nbsp;指定的端口。&nbsp;&nbsp;<strong>New in 3.3.0</strong></td>
</tr>
<tr>
<td><strong>minSessionTimeoutmaxSessionTimeout</strong></td>
<td>Session超时时间限制，如果客户端设置的超时时间不在这个范围，那么会被强制设置为最大或最小时间。默认的Session超时时间是在2 *&nbsp;&nbsp;<strong>tickTime ~ 20 * tickTime&nbsp;这个范围&nbsp;New in 3.3.0</strong></td>
</tr>
<tr>
<td><strong>fsync.warningthresholdms</strong></td>
<td>事务日志输出时，如果调用fsync方法超过指定的超时时间，那么会在日志中输出警告信息。默认是1000ms。(Java system property:&nbsp;&nbsp;<strong>fsync.warningthresholdms</strong>&nbsp;)<strong>New in 3.3.4</strong></td>
</tr>
<tr>
<td><strong>leaderServes</strong></td>
<td>默认情况下，Leader是会接受客户端连接，并提供正常的读写服务。但是，如果你想让Leader专注于集群中机器的协调，那么可以将这个参数设置为no，这样一来，会大大提高写操作的性能。(Java system property: zookeeper.&nbsp;<strong>leaderServes</strong>&nbsp;)。</td>
</tr>
</tbody>
</table>
<table style="height: 176px; width: 585px;" border="0">
<tbody>
<tr>
<td><strong>server.x=[hostname]:nnnnn[:nnnnn]</strong></td>
<td>这里的x是一个数字，与myid文件中的id是一致的。右边可以配置两个端口，第一个端口用于F和L之间的数据同步和其它通信，第二个端口用于Leader选举过程中投票通信。&nbsp;&nbsp;<br />(No Java system property)</td>
</tr>
<tr>
<td><strong>group.x=nnnnn[:nnnnn]weight.x=nnnnn</strong></td>
<td>对机器分组和权重设置，可以&nbsp;&nbsp;<a href="http://zookeeper.apache.org/doc/r3.4.3/zookeeperHierarchicalQuorums.html" target="_blank">参见这里</a></td>
</tr>
<tr>
<td><strong>cnxTimeout</strong></td>
<td>Leader选举过程中，打开一次连接的超时时间，默认是5s。</td>
</tr>
<tr>
<td><strong>zookeeper.DigestAuthenticationProvider<br />.superDigest</strong></td>
<td>ZK权限设置相关，具体参见&nbsp;&nbsp;<a href="http://nileader.blog.51cto.com/1381108/930635" target="_blank">《&nbsp;&nbsp;<strong>使用super</strong>&nbsp;&nbsp;<strong>身份对有权限的节点进行操作&nbsp;</strong>》&nbsp;</a>&nbsp;和&nbsp;&nbsp;<a href="http://nileader.blog.51cto.com/1381108/795525" target="_blank">《&nbsp;<strong>ZooKeeper</strong>&nbsp;&nbsp;&nbsp;<strong>权限控制&nbsp;</strong>》</a></td>
</tr>
<tr>
<td><strong>skipACL</strong></td>
<td>对所有客户端请求都不作ACL检查。如果之前节点上设置有权限限制，一旦服务器上打开这个开头，那么也将失效</td>
</tr>
<tr>
<td><strong>forceSync</strong></td>
<td>这个参数确定了是否需要在事务日志提交的时候调用&nbsp;<a href="http://rdc.taobao.com/team/%5C/java%5C/jdk1.6.0_22%5C/jre%5C/lib%5C/rt.jar%3Cjava.nio.channels(FileChannel.class%E2%98%83FileChannel" target="_blank">FileChannel&nbsp;</a>.force来保证数据完全同步到磁盘。</td>
</tr>
<tr>
<td><strong>jute.maxbuffer</strong></td>
<td>每个节点最<a class="replace_word" title="Hadoop知识库" href="http://lib.csdn.net/base/20" target="_blank">大数据</a>量，是默认是1M。这个限制必须在server和client端都进行设置才会生效。</td>
</tr>
<tr>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
</tbody>
</table>