# 如何在 Ubuntu 20.04 上安装 Hadoop LTS[Focal Fossa]-elder node

> 原文：<https://blog.eldernode.com/install-hadoop-on-ubuntu-20-04/>

![How To Install Hadoop On Ubuntu 20.04 LTS [Focal Fossa]](img/c580747c2278819e02124e297c4df731.png)

了解如何**一步步在 Ubuntu 20.04 LTS** 上安装 Hadoop。Hadoop 是一个基于 java 的数据框架。这款开源软件为任何类型的数据提供了海量存储、强大的处理能力，以及处理几乎无限的并发任务或工作的能力。购买您自己的 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 并加入我们这篇文章，开始了解它并享受 Hadoop 最重要的能力，快速存储和处理大量的任何类型的数据。

如果您考虑以下**先决条件:**，本教程可能会更有用

**1-** 运行 Ubuntu 20.04 的服务器。

**2-** 本地/远程机器上的 Sudo 或 root 权限

**3-** 16GB 内存/8vCPU/20GB 引导磁盘/100GB 原始磁盘，用于数据存储

## 教程在 Ubuntu 20.04 LTS 上安装 Hadoop

Hadoop 以其计算能力而闻名。当你使用它的时候，你明白你使用的计算节点越多，你的处理能力就越强。通过处理从公司收集的数据，Hadoop 可以推断出未来的决策结果。

## 在 Ubuntu 20.04 上安装和配置 Apache Hadoop

让我们通过本教程的 10 个步骤来完成对 Hadoop 的学习。

### 第一步:如何安装 Java

由于 Hadoop 是用 [Java](https://blog.eldernode.com/install-java-apt-ubuntu-20/) 编写的，所以可以在更新系统后从默认的 apt 仓库安装 OpenJDK 8:

```
sudo apt update
```

```
sudo apt install openjdk-8-jd
```

注意:Hadoop 支持 Java 版本 8

运行以下命令检查 Java 的安装版本:

```
java -version
```

### 步骤 2:如何创建 Hadoop 用户

考虑到[安全](https://blog.eldernode.com/tag/security/)的原因，建议您创建一个单独的用户来运行 Hadoop。为此，键入以下名为 Hadoop 的命令:

```
sudo adduser hadoop
```

### 步骤 3:如何配置基于 SSH 密钥的认证

在此步骤中，您需要为本地系统配置无密码 SSH 验证。因此，使用您在上面的步骤中创建的用户 **hadoop** 登录，并运行下面的命令:

```
su - hadoop
```

要生成公钥和私钥对，请键入:

```
ssh-keygen -t rsa
```

```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

```
chmod 640 ~/.ssh/authorized_keys
```

然后，使用以下命令验证无密码 SSH 身份验证:

### 第四步:在 Ubuntu 20.04 上安装 Hadoop

确保使用用户 **hadoop** 登录后，运行以下命令

```
su - hadoop
```

要下载最新版本的 Hadoop，请键入:

下载完成后，使用以下命令提取下载的文件:

```
tar -xvzf hadoop-3.3.0.tar.gz
```

使用以下命令将提取的目录重命名为 hadoop:

要在系统上配置 Hadoop 和 Java 环境变量，请打开文件~/。bashrcin 在您最喜欢的文本编辑器中:

```
nano ~/.bashrc
```

然后，添加以下几行:

```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/  export HADOOP_HOME=/home/hadoop/hadoop  export HADOOP_INSTALL=$HADOOP_HOME  export HADOOP_MAPRED_HOME=$HADOOP_HOME  export HADOOP_COMMON_HOME=$HADOOP_HOME  export HADOOP_HDFS_HOME=$HADOOP_HOME  export HADOOP_YARN_HOME=$HADOOP_HOME  export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native  export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin  export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

现在，您可以保存并关闭文件。

要激活环境变量，请运行:

```
source ~/.bashrc
```

然后，打开 Hadoop 环境变量文件:

```
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```

根据 JAVA 安装路径，取消注释并更改变量 JAVA _:

```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/
```

现在，完成后您可以保存并关闭文件。

### 第 5 步:如何配置 Hadoop

要创建这两个目录，请运行:

```
mkdir -p ~/hadoopdata/hdfs/namenode
```

```
mkdir -p ~/hadoopdata/hdfs/datanode
```

然后，您可以编辑文件 core-site.xml 并使用您的系统主机名进行更新:

```
nano $HADOOP_HOME/etc/hadoop/core-site.xml
```

接下来，您应该编辑以下文件以匹配系统的主机名:

```
<configuration>          <property>                 <name>fs.defaultFS</name>                 <value>hdfs://example.com:9000</value>          </property>  </configuration>
```

现在，您可以保存并关闭文件。然后，编辑文件 **hdfs-site.xml** :

```
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

目录路径 **NameNode** 和 **DataNode** 应该修改如下:

```
<configuration>             <property>                  <name>dfs.replication</name>                  <value>1</value>          </property>             <property>                  <name>dfs.name.dir</name>                  <value>file:///home/hadoop/hadoopdata/hdfs/namenode</value>          </property>             <property>                  <name>dfs.data.dir</name>                  <value>file:///home/hadoop/hadoopdata/hdfs/datanode</value>          </property> 
```

现在，您可以保存并关闭文件。然后，编辑文件 **mapred-site.xml** :

```
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

进行以下更改:

```
<configuration>          <property>                  <name>mapreduce.framework.name</name>                  <value>yarn</value>          </property>  </configuration>
```

现在，您可以保存并关闭文件。然后，编辑 y 文件 **arn-site.xml** :

```
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

最后，进行以下更改:

```
<configuration>          <property>                  <name>yarn.nodemanager.aux-services</name>                  <value>mapreduce_shuffle</value>          </property>  </configuration>
```

现在，完成后您可以保存并关闭文件。

### 第六步:如何启动 Hadoop 集群

首先，您应该将 Namenode 格式化为 **hadoop** 用户。使用下面的命令来完成此操作:

```
hdfs namenode -format
```

要启动 **hadoop** 集群，运行:

```
start-dfs.sh
```

然后，键入以下命令来启动服务 **YARN**

```
start-yarn.sh
```

使用下面的命令检查所有服务的状态 **Hadoop**

```
jps
```

### 第七步:如何配置防火墙

通过运行以下命令，允许 Hadoop 连接通过防火墙:

```
ufw allow 9870/tcp
```

```
ufw allow 8088/tcp
```

### 步骤 8:如何登录 Hadoop Namenode 和资源管理器

您可以访问网址“http://example.com:9870”来访问 Namenode。在那里，您可以看到服务摘要屏幕。

此外，要访问资源管理，您应该访问 URL“http://example . com:8088”以查看 Hadoop 管理屏幕。

### 第九步:如何检查 Hadoop 集群

虽然 Hadoop 集群已经安装并配置完毕，但是您需要在 HDFS 文件系统中创建一些目录来测试 Hadoop。

*注意*:请确保使用用户 **hadoop** 登录

```
su - hadoop
```

使用以下命令在 HDFS 文件系统中创建一个目录:

```
hdfs dfs -mkdir /test1
```

```
hdfs dfs -mkdir /logs
```

要列出上面的目录，请键入:

然后尝试把一些文件放到 hadoop 的文件系统中。您可以将主机上的日志文件添加到 hadoop 文件系统中。

```
hdfs dfs -put /var/log/* /logs/
```

注意:如果您需要检查上述文件和目录，您可以在 Hadoop Namenode web 界面中完成。

进入 Namenode web 界面，点击**工具- >浏览文件系统**。您应该会看到您之前创建的目录。

### 第十步:如何停止 Hadoop 集群

您需要作为 Hadoop 用户运行 **stop-dfs.sh** 和 **stop-yarn.sh** 脚本，以便能够停止 Hadoop Namenode 和 yarn 服务

运行以下命令停止 Hadoop Namenode 服务:

```
stop-dfs.sh
```

要停止 Hadoop 资源管理器服务，请键入:

```
stop-yarn.sh
```

## 结论

在本文中，您已经学习了如何在 Ubuntu 20.04 LTS 上安装 Hadoop。由于您的业务需求的种类，您可以通过在您的组织中采用这种技术来使用它在当今生活中的重要作用，因为它适用于任何领域。