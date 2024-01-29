# Redis最佳实践
## 服务端的优化
### 持久化配置
Redis持久化虽然可以保证数据安全，但是会带来额外的开销，因此：
- 用于做缓存的Redis实例尽量不要开启持久化功能
- 建议关闭RDB持久化功能，使用AOF持久化
- 利用脚本定期在slave节点做RDB，实现数据备份
- 设置合理的rewrite阈值，避免频繁的bgrewrite
- 配置no-appendfsync-on-rewrite = yes，禁止在rewrite期间做AOF
部署的有关建议
- Redis实例的物理机器要预留足够的内存，应对fork和rewrite
- 单个Redis实例内存上限不要太大，可以加快fork的速度、减少主从同步、数据迁移的压力
- 不要与CPU密集型应用部署在一起
- 不要与高硬盘负载应用部署在一起，例如：数据库、消息队列

### 慢查询
在Redis执行耗时超过某个阈值的命令；阈值设置方式
`slowlog-log-slower-than` 建议是1000
`slowlog-max-len`慢查询日志，默认是128，建议1000
修改两个配置可以使用config set命令
![[Pasted image 20231210220828.png]]

如何找到慢查询：
slowlog len 多少个慢查询
slowlog get

### 命令及安全配置
Redis的漏洞原因：
- Redis未设置密码
- 利用Redis的config set命令动态修改Redis配置
- 使用了Root账号权限启动Redis

为了避免漏洞，建议：
- Redis一定要设置密码
- 禁止线上使用下列命令：keys、flushall、flushdb、config set命令，使用rename-command来实现：![[Pasted image 20231210222139.png]]
- bind，限制网卡
- 开启防火墙
- 不要使用Root账号启动Redis
- 不要使用默认端口6379

### 内存配置
当Redis内存不足时，可能导致Key频繁被删除，响应时间变长，QPS不稳定等问题。当内存使用率达到90%以上时就需要警惕，并快速定位到内存占用的原因
![[Pasted image 20231210222547.png]]

Info memory
memory xxxx 【status】

### 内存缓冲区配置
#### 复制缓冲区
主从复制时候的repl_backlog_buf，如果太小那么可能会导致频繁的全量复制，影响性能，因此可以通过repl_backlog_size来设置，默认为1MB
#### AOF缓冲区
AOF刷盘之前的缓冲区，AOF执行rewrite的缓冲区，无法设置容量上限
#### 客户端缓冲区
分为输入缓冲区和输出缓冲区，输入缓冲区最大1G且不能设置，输出缓冲区可以设置
![[Pasted image 20231210223630.png]]
## 集群最佳实践
### 集群完整性问题
在Redis的默认配置中，如果发现任何一个插槽不能用，则整个集群都会停止对外服务：
![[Pasted image 20231210224015.png]]
为了保证集群的高可用，建议将此设置项设置为no
cluster nodes 命令

### 集群带宽问题
集群节点之间会通过相互ping来确定集群中其他节点的状态。每次ping都要携带信息
- 插槽信息
- 集群状态信息
集群中节点越多，集群状态信息越大，每次互通所需要的带宽就会越高

解决途径：
- 避免大集群，集群节点数不要太多，最好少于1000，如果业务庞大，就建立多个集群
- 避免在单个物理机中运行太多Redis实例
- 配置合适的Cluster-node-timeout 值
![[Pasted image 20231210225030.png]]


