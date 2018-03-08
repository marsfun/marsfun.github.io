QJM方式实现HDFS HA

## 注意 journalnode 存储edits，个数为奇数个,如：3、5个  
- zkfc 作为自动namenode切换的辅助管理进程，手工切换可以不用zkfc  
- zkfc 也可能有brain-split，只能设置 sshfence 尽量避免。



### 1. start journalnode on slave nodes
```bash
hadoop-daemon.sh start journalnode
```
- num: odd 部署奇数个
### 2. format primary namenode and start it. (e.g. nn1) 
```bash 
hdfs namenode -format  
```
已经存在数据，会提示“是否re-format”
```bash
hadoop-daemon.sh --script hdfs start namenode
```
### 3. sync namenode metadata to nn2 (primary: nn1, standby:nn2)  
```bash
hdfs namenode -bootstrapStandby  
```

已经存在数据，会提示“是否re-format”

```bash
hadoop-daemon.sh --script hdfs start namenode
```
#### A. 检测两个namanode 应都是standby
```bash
hdfs haadmin -getServiceState nn1
hdfs haadmin -getServiceState nn2
```
### 4.1. 手工激活namenode
- 手动激活一个namnode  
```bash
hdfs haadmin -transitionToActive nn2
```

### 4.2. 自动监控namenode （zkfc）

- 在任一namenode上执行zk 信息的format命令  
```bash
hdfs zkfc -formatZK  
```
- 在 __所有__ namenode上启动zkfc监控进程  
```bash
hadoop-daemon.sh --script hdfs start zkfc  
```
#### B. 测试namenode failover
- 在nn1  
```bash
kill -9 pid （namenode） 
```
- 查看 nn1 应该为不可见，nn2为 active  
```bash
hdfs haadmin -getServiceState nn1  
hdfs haadmin -getServiceState nn2
  
yarn rmadmin -getServiceState rm1
yarn rmadmin -getServiceState rm2
```