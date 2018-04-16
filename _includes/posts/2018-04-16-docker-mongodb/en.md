## centos7下Docker安装与卸载

[^注1]: Linux 内核需要3.10以上
[^注2]: 以下操作都以管理员账户运行

### Docker的安装

#### 更新yum到最新

```shell
yum update 
```



#### 修改相关配置文件

```shell
tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF 
```



#### 安装

```shell
yum install docker-engine
```



#### 启动

```shell
service docker start
```



#### 检查是否安装成功

```shell
docker  version 
```



### Docker的卸载

#### 列出你安装过的包

```shell
yum list installed | grep docker
docker-engine.x86_64       17.05.0.ce-1.el7.centos
                                               @dockerrepo                      
docker-engine-selinux.noarch
                                               @dockerrepo     
```

#### 删除安装包

```shell
yum -y remove docker-engine.x86_64
yum -y remove docker-engine-selinux.noarch
```

#### 删除镜像/容器等

```shell
rm -rf /var/lib/docker
```



##### 参考：

[Docker的安装](https://blog.csdn.net/chenaima1314/article/details/79771831)

[Centos系统下docker的安装与卸载](https://blog.csdn.net/a527219336/article/details/50800181)