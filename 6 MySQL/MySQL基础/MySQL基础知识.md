## 存储引擎



#### InnoDB

![image-20220115195940741](MySQL%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.assets/image-20220115195940741.png)

- 支持事务
- MySQL默认使用InnoDB作为存储引擎
- 除非需要用到 InnoDB 不具有的特性，否则绝大多数情况最好选用 InnoDB存储引擎
- 采用 MVCC 支持高并发
- 实现了4个标准的隔离级别（读未提及，读提交，可重复读，串行化），并默认级别是可重复读（REPEATABLE READ), 并且在可重复读的级别下，通过 MVCC+Next-Key Locking防止幻读
- 主索引时聚簇索引，在索引中保存了数据，从而避免了直接读取磁盘，因此对主键查询有很高的性能
- InnoDB内部做了很多优化：
  - 包括从磁盘读取数据时采用可预测性读
  - 能够自动在内存中创建hash索引以加速读操作的自适应哈希索引
  - 以及能够加速插入操作的插入缓冲区等
- InnoDB 支持真正的在线热备份（MySQL 其他的存储引擎不支持在线热备份，要获取一致性视图需要停止对所有表的写入，而在读写混合的场景中，停止写入可能也意味着停止读取）



#### MyISAM

![image-20220115200907166](MySQL%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.assets/image-20220115200907166.png)

#### InnoDB 和 MyISAM 的比较



![image-20220115201116924](MySQL%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.assets/image-20220115201116924.png)



## 索引





## 来源，链接

来自 敖丙

https://mp.weixin.qq.com/s/J3kCOJwyv2nzvI0_X0tlnA















