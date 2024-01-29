# 数据结构

## 动态字符串SDS（simple Dynamic String）
Redis构建了一种新的字符串结构；
![[Pasted image 20231210230340.png]]
字符串会自动扩展，然后会动态扩容；
![[Pasted image 20231210230704.png]]

## IntSet
数据量不太多的时候使用；
整数的集合；是Redis中set集合的一种实现方式，基于整数数组来实现，并且具备长度可变、有序等特征。
结构如下：
![[Pasted image 20231210231014.png]]

支持升级操作，就是如果一个元素的编码变大，其他元素的编码也要升级，这样才能保持一致；
![[Pasted image 20231210231946.png]]

## Dict
### 结构
![[Pasted image 20231210233407.png]]
![[Pasted image 20231210233544.png]]
![[Pasted image 20231210233617.png]]
### Dict的扩容
集合中元素较多时，hash冲突会加剧，因此要考虑扩容；
Dict在每次新增键值对时都会检查负载因子（LoadFactor = used/size)，满足以下两种情况时会触发哈希表扩容；
- 哈希表的loadFactor >= 1,并且服务器没有执行bgsave或者bgrewrite等后台进程
- 哈希表的loadFactor > 5

### Dict的收缩
每次删除元素时，会检查负载因子，负载因子<0.1的时候就收缩
哈希表的初始size是4；
![[Pasted image 20231210235417.png]]

## ZipList
压缩链表；
各种省内存；
对节点的个数是有限制的；

### ZipList的连锁更新问题
其实就是相当于保存长度的字节数扩展了，因此需要更新；
![[Pasted image 20231211001849.png]]

## QuickList

P152

# 网络模型



# 通信协议



# 内存策略


