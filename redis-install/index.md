# Redis安装


## 下载

官网下载最新版本的安装包

```language-bash
  cd /usr/local
  wget http://download.redis.io/releases/redis-5.0.5.tar.gz
```

[历史版本库地址](http://download.redis.io/releases/)

<http://download.redis.io/releases/>



## 安装

### **第一步**：解压

```language-bash
  cd /usr/local
  tar -zxvf redis-5.0.5.tar.gz
  mv redis-5.0.5 redis
```

### **第二步**：安装 gcc 环境 

```language-bash
  yum install gcc-c++
```

### **第三步**：编译

```language-bash
  cd /usr/local/redis
  make
```

### **第四步**：安装

```language-bash
  cd /usr/local/redis/src
  make install
```



## 配置

### **第一步**：修改启动脚本

1. #### 修改redis内置的启动脚本

  &emsp;&emsp;&emsp;&emsp;![img](https://raw.githubusercontent.com/lrioo/blog_image/master/image/posts/redis_install/01.png)

  &emsp;&emsp;将图中的内容修改为当前部署环境的真实路径即可。

```language-bash
  vim /usr/local/redis/utils/redis_init_script
```


2. #### 将redis_init_script拷贝到/etc/init.d目录下，并重命名为redis

  ```language-bash
    cp /usr/local/redis/utils/redis_init_script /etc/init.d/redis
  ```

### **第二步**：修改配置文件

```language-bash
 vim /usr/local/redis/redis.conf
```

1. #### 注释掉 bind 127.0.0.1 这一行（解决只能特定网段连接的限制）

2. #### 将 protected-mode 属性改为 no （关闭保护模式，不然会阻止远程访问）

3. #### 将 daemonize 属性改为 yes （这样启动时就在后台启动）

4. #### 设置密码（可选，个人建议还是设个密码）

### **第三步**：创建配置文件目录，并迁移文件

```language-bash
  mkdir -p /etc/conf/redis
  cp /usr/local/redis/redis.conf /etc/conf/redis/6379.conf
```
文件名对应端口号

### **第四步**：设置开机启动

\- 打开redis命令: service redis start

\- 关闭redis命令: service redis stop

\- 设为开机启动: chkconfig redis on

\- 设为开机关闭: chkconfig redis off



## 相关问题

#### **安装Redis，执行make test时遇到You need tcl 8.5 or newer in order to run the Redis test**

&emsp;&emsp;![img](https://raw.githubusercontent.com/lrioo/blog_image/master/image/posts/redis_install/02.png)

#### **解决方案**：安装tcl

```language-bash
 yum install tcl
```



#### **dump.rdb 文件位置**

1. 查看dump.rdb的位置

```language-bash
 find / -name dump.rdb
```

​&emsp;&emsp;&emsp;显示为`/dump.rdb`

2. 查看当前的配置文件`/etc/conf/redis/6379.conf`，其中dir配置信息是`dir ./`

3. 停止redis服务

4. 将`dir ./`更改为`dir /data/redis`

5. 创建文件夹，并将dump.rdb文件移动到相应的文件夹下

```language-bash
  mkdir -p /data/redis
  mv /dump.rdb /data/redis/
```

6. 启动redis服务



## 参考

#### **安装**

<https://blog.csdn.net/gs_albb/article/details/92022693>

<https://blog.csdn.net/qq_36737803/article/details/90578860>

<https://www.cnblogs.com/therunningfish/p/9535022.html>

  

#### **相关问题**

<https://www.cnblogs.com/zhaoshunjie/p/5907029.html>

<https://blog.csdn.net/shn1994/article/details/86527283>
