# My Blog
## 文档模板记录
- **操作文档** ➜ 指南式结构（What / How / Example / 注意事项）
- **原理文档** ➜ 深度结构（Why / What’s Inside / How It Works / Source Trace）
- 工具使用类文档
    ```bash
    # 文档标题（例如：iptables 使用详解）

    ## 一、背景介绍
    简要说明该技术的用途、为何要写这篇文档。

    ## 二、基本概念
    解释核心术语、基础概念（如 Netfilter、链、规则）。

    ## 三、安装与环境准备
    - 系统要求
    - 安装方式（如 apt/yum）
    - 启用服务/检查是否可用

    ## 四、基本用法示例
    - 示例 1：查看现有规则
    - 示例 2：允许特定端口
    - 示例 3：禁止 IP
    - 示例 4：保存/恢复规则

    ## 五、高级用法
    - NAT 转发
    - 多表配置
    - 配合 fail2ban 使用

    ## 六、排查与常见问题
    - 配置不生效？
    - 系统重启后规则丢失？
    - 防火墙冲突？

    ## 七、总结与建议
    - 推荐的配置策略
    - 常用命令汇总表

    ## 八、参考资料
    - 官方文档
    - 实战文章/博客链接
    ```
- 原理类技术文档
    ```bash
    # 文档标题（例如：iptables 工作原理详解）

    ## 一、背景说明
    - 为什么写这篇文档？
    - 这个工具的地位与作用？

    ## 二、整体架构概览
    - 相关模块图 / 数据流图
    - 哪些核心概念？

    ## 三、关键机制解析
    ### 3.1 Netfilter 与 iptables 的关系
    ### 3.2 四个表的处理流程
    ### 3.3 数据包在内核中的流转路径（结合 Hook 点）

    ## 四、工作流程详解
    - 数据包进入 INPUT 链时的处理过程
    - NAT 与转发时的路径与 Hook 顺序

    ## 五、性能与行为特性
    - iptables 匹配顺序
    - 性能瓶颈在哪？
    - 多核并发情况？

    ## 六、与相关组件的关系
    - nftables 的演进
    - 与 firewalld 的关系
    - 与内核 netfilter 模块的接口调用关系

    ## 七、源码入口简析（可选）
    - iptables 命令工具代码路径
    - Netfilter 模块在 kernel 源码中的位置

    ## 八、小结与参考
    - 总结机制特点
    - 学习推荐资料
    ```
- 部署运维类 / 环境搭建类技术文档
    ```bash
    # 💻 项目部署文档模板（以 Hadoop 为例）

    ## 一、项目概览

    - **名称**：Hadoop 分布式集群部署  
    - **目标**：完成在 3 台服务器上部署 Hadoop 分布式集群  
    - **适用人员**：研发、测试、运维  
    - **文档状态**：✅已验证 / 🚧待完善 / 📝草稿

    ---

    ## 二、环境准备

    ### 2.1 硬件规划

    | 节点名     | IP地址         | 角色           | 备注     |
    |------------|----------------|----------------|----------|
    | master01   | 192.168.1.10   | NameNode       | 主节点   |
    | worker01   | 192.168.1.11   | DataNode       | 从节点 1 |
    | worker02   | 192.168.1.12   | DataNode       | 从节点 2 |

    ### 2.2 软件依赖

    - 操作系统：CentOS 7.9 / Ubuntu 22.04  
    - JDK：OpenJDK 1.8+  
    - Hadoop：3.3.6  
    - 其他工具：ssh、scp、vim、curl

    ---

    ## 三、安装步骤

    ### 3.1 创建用户并设置权限

    ```bash
    sudo useradd hadoop
    sudo passwd hadoop
    sudo visudo
    # 添加：hadoop ALL=(ALL) NOPASSWD:ALL
    ```

    ### 3.2 安装 Java

    ```bash
    sudo apt install -y openjdk-8-jdk
    java -version
    ```

    ### 3.3 配置主机名和 hosts 文件

    ```bash
    sudo vim /etc/hosts
    # 添加以下内容：
    192.168.1.10 master01
    192.168.1.11 worker01
    192.168.1.12 worker02
    ```

    ### 3.4 SSH 免密登录配置

    ```bash
    ssh-keygen -t rsa
    ssh-copy-id hadoop@master01
    ssh-copy-id hadoop@worker01
    ssh-copy-id hadoop@worker02
    ```

    ---

    ## 四、部署 Hadoop

    ### 4.1 解压并设置环境变量

    ```bash
    tar -xzf hadoop-3.3.6.tar.gz -C /opt/
    vim ~/.bashrc
    # 添加：
    export HADOOP_HOME=/opt/hadoop-3.3.6
    export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
    source ~/.bashrc
    ```

    ### 4.2 配置文件修改

    #### core-site.xml

    ```xml
    <configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://master01:9000</value>
    </property>
    </configuration>
    ```

    #### hdfs-site.xml

    ```xml
    <configuration>
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    </configuration>
    ```

    #### workers

    ```bash
    worker01
    worker02
    ```

    ---

    ## 五、启动与验证

    ### 5.1 初始化

    ```bash
    hdfs namenode -format
    ```

    ### 5.2 启动服务

    ```bash
    start-dfs.sh
    ```

    ### 5.3 验证进程和 UI

    ```bash
    jps
    # Master 应包含：NameNode、SecondaryNameNode
    # Worker 应包含：DataNode
    ```
    - Web UI 访问：http://master01:9870/

    ---

    ## 六、常见问题与排查

    | 问题                            | 解决方案                                      |
    |---------------------------------|-----------------------------------------------|
    | ssh 报错 `Permission denied`   | 检查 ssh 免密配置                             |
    | Hadoop 启动失败                | 查看日志 `$HADOOP_HOME/logs`                  |
    | NameNode 页面无法访问          | 检查防火墙、端口是否被占用或未监听            |

    ---

    ## 七、附录

    ### 7.1 文件目录结构

    ```bash
    /opt/hadoop-3.3.6/
    ├── bin
    ├── etc/hadoop
    ├── logs
    ├── sbin
    ```

    ### 7.2 官方文档参考

    - https://hadoop.apache.org/
    - https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html

    ```