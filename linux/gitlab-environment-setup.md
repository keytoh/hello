# Linux环境安装Gitlib

## 下载

```SHELL
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el6/gitlab-ce-12.4.2-ce.0.el6.x86_64.rpm
```

## 安装

```SHELL
rpm -i gitlab-ce-12.4.2-ce.0.el6.x86_64.rpm
```

## 配置

```SHELL
# 编辑指定配置文件
vi /etc/gitlab/gitlab.rb

# 修改外部访问地址
external_url 'http://<hostname>:<port>'
```

## 重载配置及启动
```SHELL
# 重载配置
gitlab-ctl reconfigure
# 启动服务
gitlab-ctl restart
```

## 配置防火墙

```SHELL
# 端口
firewall-cmd --zone=public --add-port=<port>/tcp --permanent
firewall-cmd --reload
```

