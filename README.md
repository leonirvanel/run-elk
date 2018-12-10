# elk-run
记录elk的坑

环境描述：集群由node-a、node-b、node-c三台机器组成。每个node既是master节点，又是data节点。

故障一：node-a宕机，导致Logstash-today不可写，集群变为red
原因：当前设置副本数为0。node-a保存了约1/3的index的主副本。当node-a宕机，这个主副本分布在这台机器上的index不可操作了。
修复：将副本数设置为1。

故障二：node-a的elasticsearch进程退出
原因：底层ceph“闪断”，导致IO ERROR。
修复：暂时无法修复

故障三：node-a宕机重启，恢复缓慢
原因：node-a owner的indes，逐渐提升到1/3
