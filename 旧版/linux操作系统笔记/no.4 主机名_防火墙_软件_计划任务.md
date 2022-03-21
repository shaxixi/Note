[toc]

# 主机名

> `#hostname`
>
> `#hostname -f` FQDN(全限定域名)

* 临时设置主机名（需要切换用户 `su 用户名` 使之生效）
  `#hostname 设置的新主机名`

* 永久设置主机名（需要重启）

  1. 主机名的配置文件：`/etc/sysconfig/network`
  2. 修改其中的 `HOSTNAME`为自己需要设置的永久主机名
  3. 设置FQDN，修改Linux服务器的hosts文件 `/etc/hosts` ,将新主机名加入到 第一行的`localhost` 前面

  * 不设置FQDN会怎么样？
    * **很多开源服务器软件无法启动或者报错**
    * 看到主机名对其作用有一个初步判断
    * 如果不设置则会影响本地的域名的解析（本地访问）

# chkconfig

（提供“开机启动项”的一个管理服务）

* 开机启动服务查询：`#chkconfig --list`
  
  > 0-6表示各个启动级别
  
* 删除服务：`#chkconfig --del 服务名`
* 添加开机启动服务：`#chkconfig --add 服务名` （必须要保证服务正常运行，才可以添加）
* 设置服务在某个级别下开机启动/不启动：`#chkconfig --level 连在一起的启动级别 服务名 on/off`

# ntp服务

（主要是用于对计算机的时间同步管理操作）

> 1. 时间对服务器来说是很重要的，一般很多网站都需要读取服务器时间来记录相关信息，如果时间不准，则可能造成很大的影响
> 2. 上游的概念（时间校对的方式）：自己服务器 -> 上一级别的时间服务器 -> 上上一级的时间服务器 -> 根时间服务器

* 一次性同步时间：
  * `#ntpdata 时间服务器的域名或者IP地址`
  * IP地址查看可以访问：http://www.ntp.org.cn/pool.php
* 设置时间同步服务：
  * `#service ntpd start`
  * `/etc/init.d/ntpd start`

# 防火墙服务

（防范一些网络攻击，有软件防火墙和硬件防火墙之分）

> 防火墙（iptables)/(firewalld)选择让请求通过，从而保证网络安全性。

* 查看iptables是否开机启动： `chkconfig --list | grep iptables`
* iptables 服务启动/重启/关闭
  * `#service iptables start/restart/stop`
  * `/etc/init.d/iptables start/restart/stop`  (是一个指令，前面的路径不能省略)
* 查看iptables的状态（规则）
  * `# service iptables status`
  * 如果iptables没有启动，则提示服务没有启动，如果已经启动，则显示防火墙的相关的规则信息
  
* 查看规则的命令

  * `#iptables -L -n`

    > `-L`： 表示列出规则
    >
    > `-n`: 表示将单词表达形式改成数字形式显示

* 简单设置防火墙规则

  * 例如：

    * 需要允许80端口通过防火墙，则规则可以用以下的命令来设置

      `#iptables -A INPUT -p tcp --dport 80 -j ACCEPT`  允许访问80端口

      > `iptables` : 主命令
      >
      > `-I` ： 表示将规则放到（最前）
      >
      > `-A` ： add,添加规则（最后）
      >
      > `INPUT` ： 进站请求 【出站： `output`】
      >
      > `-p` : protocol,指定协议 `icmp/tcp/udp`
      >
      > `--dport` : 指定端口号
      >
      > `-j` : 指定行为结果，允许`accept`/禁止`reject`

    * 添加完成之后需要保存操作：
      `/etc/init.d/iptables save`
    
  * 例如：

    * `iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT`  
      #允许本地回环接口（即运行本机访问本机）
    * `iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`
      #允许已建立的或相关连的通行
    * `iptables -A OUTPUT -j ACCEPT`
      #允许所有本机向外的访问
    * `iptables -A INPUT -p tcp --dropt 22 -j ACCEPT`
      #允许访问22端口
    * `iptables -A INPUT -p tcp --dropt 80 -j ACCEPT`
      #允许访问80端口
    * `iptables -A INPUT -p tcp --dropt 21 -j ACCEPT`
      #允许ftp服务的21端口
    * `iptables -A INPUT -p tcp --dropt 20 -j ACCEPT`
      #允许ftp服务的20端口
    * `iptables -A INPUT -j REJECT`
      #禁止其他未允许的规则访问
    * `iptables -A FORWARD -j REJECT`
      #禁止其他未允许的规则访问

# rpm 管理 （软件管理）

（rpm类似于Windows上的电脑管家中“软件管理”，安全卫士里面“软件管家”等产品，主要作用是对Linux服务器上的软件包进行对应管理操作，管理分为：查询，卸载，安装。）

* 查询
  `#rpm -qa | grep 关键词`

  > `-q`: 查询，query
  >
  > `-a`: 全部，all

  * 查询Linux上是否安装Firefox  `#rpm -qa | grep firefox`
  * 查询是否安装qq  `#rpm -qa | grep qq`

* 卸载

  * 没有存在依赖关系的时候
    `#rpm -e 软件的名称` 
  * 当存在依赖关系的时候又不想去解决这个问题的时候
    `#rpm -e 软件包名 --nodeps`

* 安装

  > 想要装软件，和window一样，先找到安装包
  >
  > 软件包的获得方式：a、官网下载  b、光盘（或者镜像文件）读取

  **此处以光盘文件为例：**

  * 查看块状设备信息： `#lsblk` (list block devices)

    > name : 名称
    >
    > size : 设备大小
    >
    > type : 类型
    >
    > mountpoint : 挂载点 （类似于window下盘符）
    
    **光盘的挂载与解挂：**

    * 解挂（umount）操作：
      `#umount当前设备的挂载点（路径）` 
      
      > 此时相当于U盘在window上已经被弹出了，但是没有拔下电脑USB接口
      
    * 挂载（mount）光盘：
      `#mount 设备原始地址 要挂载的位置路径`

      > 设备原址地址： 地址统一都在 `/dev` 下，然后根据大小确定具体name值，拼凑在一起组成原始地址，例如当前：`/dev/sr0` 为例
      >
      > 要挂载的位置路径： 挂载目录一般都在 `mnt` 下，也可以在 mnt 下建目录，此处以 `/mnt/dvd` 为例
    
  * 安装软件的命令： `#rpm -ivh 软件包完整名称`
  
    > -i : install,安装
    >
    > -v : 显示进度条
    >
    > -h : 表示以“#”形式显示进度条
  

# crontab（计划任务）

(操作系统不可能24小时都有人在操作，有些时候想在指定的时间去执行任务，如：每天夜里2点去重新启动Apache，此时不可能真有人每天夜里两点去执行命令，这时就可以交给计划任务程序去执行操作)

* `#crontab 选项`

  > -i : list，列出指定用户的计划任务列表
  >
  > -e : edit，编辑指定用户的计划任务列表
  >
  > -u : user，指定的用户名，如果不指定，则表示当前用户
  >
  > -r : remove，删除指定用户的计划任务列表

  * 列出 `#crontab -l`
    
  * 编辑计划任务（重点）
  
    > 计划任务的规则语法格式，以行为单位，一行则为一个计划：
    >
    > `分 时 日 月 周 需要执行的命令`
  
    > 取值范围：
    >
    > 分：0~59	时：0~23	日：1~31	月：1~12	周：0(星期天)~6
    >
    > 四个符号：
    >
    > *：表示取值范围中的每一个数字
    >
    > -：做连续区间表达式的，要想表示1~7，可以写成1-7
    >
    > /：表示每多少个，例如：每十分钟一次，可以写成*/10
    >
    > ，：表示多个取值，比如想在1点，两点，六点执行，可以写成1，2，6
  
    **例如：**
  
    * 每天的0点0分执行 reboot 指令
       `0 0 * * * reboot`
    * 每月1，10，22日的4：45重启network服务
      `45 4 1,10,22 * *  service network restart`
    * 每周六，周日的1：10重启network服务
      `10 1 * * 0,6 service network restart`
    * 每天18：00至23：00 每天隔30分钟重启network服务
      `*/30 18-23 * * * service network restart`
    * 每隔两天的上午8点到11点的第3和第15分钟执行一次重启
      `3,15 8-11 */2 * * reboot`
  
* crontab 权限问题

  * 本身是任何用户都可以创建自己的计划任务，但超级管理员可以通过配置来设置用户是否允许设置计划任务

    * 黑名单配置： `/etc/cron.deny`  （里面写用户，一行一个）
      
* 白名单配置：`/etc/cron.allow` （本身不存在，自己创建）
    
> 白名单优先级高于黑名单
    >
> 如果一个用户同时存在两个名单文件夹中，则会被默认允许创建执行计划任务。