# linux虚拟机初始化配置



## IP地址修改

```SHELL
# 切换为根用户
su root

# 编辑指定配置文件
vim /etc/sysconfig/network-scripts/ifcfg-ens # 按下tab键自动补全配置文件名

# 修改配置文件内容
[BOOTPROTO = dhcp] -> [BOOTPROTO = static]
[ONBOOT = no] -> [ONBOOT = yes]
IPADDR=192.168.127.xxx
NETMASK=255.255.255.0
GATEWAY=192.168.127.2
DNS1=192.168.127.2

# 重启服务
systemctl restart network
```


## 关闭图形界面
CentOS6
```SHELL
# 编辑指定配置文件
vim /etc/inittab

# 修改配置文件内容
# 图型启动: 5, 命令行启动: 3
id:3:initdefault:
```
CentOS7
```SHELL
# 默认启动命令行页面
systemctl set-default multi-user.target

# 默认启动图形界面
systemctl set-default graphical.target 
```

## 防火墙

### 防火墙设置

```SHELL
# 关闭防火墙
systemctl stop firewalld
# 禁用防火墙(执行禁用不会关闭正在运行的防火墙)
systemctl disable firewalld

# 启动防火墙
systemctl start firewalld
# 重启防火墙
systemctl restart firewalld
```

### 查看防火墙状态信息
```SHELL
systemctl status firewalld
```

### 查看防火墙是否开启
```SHELL
firewall-cmd --state
```

### 防火墙详细配置
```SHELL
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp

# 开放ssh以及http服务
firewall-cmd --add-service=ssh --permanent
firewall-cmd --add-service=http --permanent

# 开放22和80端口
firewall-cmd --permanent --add-port=22/tcp
firewall-cmd --permanent --add-port=80/tcp

# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp

# 重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload

# 参数解释
# 1、firwall-cmd: linux提供的操作 firewall 的工具
# 2、--permanent: 设置为持久
# 3、--add-port: 添加端口
```

## 配置yum源

```SHELL
# 进入yum源配置文件路径
cd /etc/yum.repos.d

# 获取CentOS7的yum源(同样使用RHEL7) 
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

# 备份yum源
cp CentOS-Base.repo CentOS-Base.repo.bk 
# 替换为网易的yum源
mv CentOS7-Base-163.repo CentOS-Base.repo

# 清除以前使用yum的缓存
yum clean all

# 建立一个缓存，以后方便在缓存中搜索
yum makecache
```

## 主机名操作

### 修改主机名

```SHELL
# 编辑指定配置文件
vim /etc/sysconfig/network

# 修改为自定义的主机名
# HOSTNAME=<hostname>

# 使配置立即生效
hostname <hostname>
# 重新登录后生效
exit
```

### 主机名绑定IP地址

```SHELL
# 编辑指定配置文件
vim etc/hosts

# 绑定 ip 和主机
192.168.1.111 <hostname>

# 保存退出后 验证
ping <hostname>
```


## 用户操作

### 新增用户
```SHELL
useradd <username> [-m] [-g] <groupname> <username>
    # -m 创建用户同时自动创建家目录
    # -g <group> 设置用户所属组  不加默认 以用户名作为所属组
```
### 修改密码

普通用户密码修改
```SHELL
su - root # 获取root权限
passwd <username> # 如 passwd user1 
# 输入两遍新密码
```
超级用密码修改 
```SHELL
passwd 超级用户名 # 如 passwd root
# 输入两遍新密码  
```

### 修改用户
```SHELL
/var/run/utmp
# 修改指定用户用户名 (重启后执行)
usermod -l <username> -d /home/<username> -m <username-old>
```

### 修改用户组

```SHELL
# 修改用户主组信息, 即 /etc/passwd 文件中的用户组信息(不常用) 
usermod -g <groupname> <username>

# 修改用户附属组信息, 即 /etc/group 文件中的用户组信息(常用)
usermod -G <groupname> <username>       
# 为张三添加 sudo 权限
# zhangsan 需要重新登录, 才会有 sudo 权限 
usermod -G sudo zhangsan   
```

### 删除用户

```SHELL
/var/run/utmp

# 删除指定用户 (重启后执行)
usrdel [-r] <username> # -r 同时删除家目录  

# 如果没有使用 -r, 可以进入`/home`, 手动删除家目录
rm -rf /home/<username>
```

## ping 命令
```SHELL
# 编辑指定配置文件
vim /etc/sysctl.conf

# 添加使用如下配置
net.ipv4.icmp_echo_ignore_all=0 

# 关闭使用如下配置
net.ipv4.icmp_echo_ignore_all=1

# 使其生效
sysctl -p
```

## window 文件字符集处理

windows平台上编辑后的脚本文件放到linux中, 需要使用dos2unix工具处理一下

```SHELL
dos2unix filename
```

