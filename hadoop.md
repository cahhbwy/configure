# hadoop 配置记录

> Ubuntu-gnome 16.04 + hadoop version 2.8.1

> 模拟分布式配置

## 环境变量
```bash
export HADOOP_HOME=/opt/hadoop
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="$HADOOP_OPTS -Djava.library.path=$HADOOP_HOME/lib/native"
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_INSTALL=$HADOOP_HOME
```

## 配置文件

```bash
cd $HADOOP_HOME/etc/hadoop
```

* hadoop-env.sh
```bash
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
```

* core-site.xml

```xml
<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```

* hdfs-site.xml
```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.name.dir</name>
    <value>file:///home/monk/workspace/hadoop/hdfs/namenode</value>
  </property>
  <property>
    <name>dfs.data.dir</name> 
    <value>file:///home/monk/workspace/hadoop/hdfs/datanode</value> 
  </property>
</configuration>
```

* yarn-site.xml
```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value> 
  </property>
</configuration>
```

* mapred-site.xml
```bash
cp mapred-site.xml.template mapred-site.xml
```
```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

## 测试

### 环境测试
```bash
cd ~
hdfs namenode -format
start-dfs.sh
start-yarn.sh
```

在浏览器访问Hadoop[http://localhost:50070/](http://localhost:50070/)

验证所有应用程序的集群[http://localhost:8088/](http://localhost:8088/)

### sample测试
```bash
hadoop fs -mkdir /input
hadoop fs -put $HADOOP_HOME/*.txt /input
hadoop fs -ls /input
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduceexamples-2.2.0.jar wordcount /input /ouput
```
