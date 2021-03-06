# Linux环境安装Maven

## 前提条件

**默认你已经安装好了JDK 如果没有安装JDK，请先去安装JDK**

## 下载

```SHELL
wget http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
```
## 解压
```SHELL
# 进入安装包目录 解压文件
tar zvxf apache-maven-3.6.1-bin.tar.gz
```
将解压文件放置到指定目录 
```SHELL
# 个人习惯安装到 /usr/local/maven/目录下
mkdir -p /usr/local/maven/
# 执行复制操作
cp -r apache-maven-3.6.1 /usr/local/maven/maven3.6.1
# 创建 maven 本地仓库目录
mkdir -p /usr/local/maven/maven3.6.1/repo
```
## 配置
编辑配置文件
```SHELL
vim /usr/local/maven/maven3.6.1/conf/settings.xml
```
修改本地仓库位置

```SHELL
<localRepository>/usr/local/maven/maven3.6.1/repo</localRepository>
```

修改 maven 镜像 (使用阿里镜像)

```XML
<mirror>
  <id>aliyun</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```
修改环境变量 
```SHELL
vim /etc/profile

# 在最开始添加如下内容
export MAVEN_HOME=/usr/local/maven/maven3.6.1
export PATH=$PATH:$MAVEN_HOME/bin
```

使环境变量立即生效

```SHELL
source /etc/profile
```

查看是否安装成功

```SHELL
mvn -v
```

查看 mvn 路径

```SHELL
echo $MAVEN_HOME
```

