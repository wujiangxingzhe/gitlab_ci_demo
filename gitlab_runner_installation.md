# Gitlab Runner安装

## 1. 安装方式

### 1.1 下载安装包
Gitlab Runner包下载地址：
* https://gitlab-runner-downloads.s3.amazonaws.com/latest/index.html
* 包括二进制、deb、rpm、源码等


https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
=>
下载指定版本，比如17.7版本
https://gitlab-runner-downloads.s3.amazonaws.com/v17.7.0/binaries/gitlab-runner-linux-amd64
* 这里的版本信息是，v17.7.0，不是v17.7，请注意

清华镜像站点：rpm包安装方式/或者yum源方式
https://mirrors.tuna.tsinghua.edu.cn/

但是可能无法下载最新的包



### 1.2 docker方式安装


## 2. rpm包安装
版本是17.7
https://gitlab-runner-downloads.s3.amazonaws.com/v17.7.0/rpm/gitlab-runner_amd64.rpm

* 下载包
```
wget https://gitlab-runner-downloads.s3.amazonaws.com/v17.7.0/rpm/gitlab-runner-helper-images.rpm
wget https://gitlab-runner-downloads.s3.amazonaws.com/v17.7.0/rpm/gitlab-runner_amd64.rpm
```

* 安装包
```
rpm -ivh gitlab-runner-helper-images.rpm
rpm -ivh gitlab-runner_amd64.rpm
``` 

* 启动 & 检查
```
gitlab-runner start
gitlab-runner status
```

