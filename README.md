# elk-run
## 一、推荐实践
  ### 优化查询性能
  差异化冷数据与热数据的副本数。最近3天的index的副本设置为2；超过3天的index的副本数设置为1。

## 二、踩过的坑
环境描述：集群由node-a、node-b、node-c三台机器组成。每个node既是master节点，又是data节点。

### 故障一：node-a宕机，导致Logstash-today不可写，集群变为red
#### 原因
  当前设置副本数为0。node-a保存了约1/3的index的主副本。当node-a宕机，这个主副本分布在这台机器上的index不可操作了。
#### 修复
  将副本数设置为1。

### 故障二：node-a的elasticsearch进程退出
#### 原因
  底层ceph“闪断”，导致IO ERROR。
#### 修复
  暂时无法修复

### 故障三：node-a宕机重启，恢复缓慢
#### 原因
node-a owner的indes，逐渐提升到1/3
#### 修复
暂无


