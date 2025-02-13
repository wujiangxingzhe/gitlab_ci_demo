# Gitlab Runner


## 1. 简介
* Gitlab Runner是开源的，用于运行作业并将结果发送回Gitlab
* 与GitlabCI结合使用，
* **Gitlab Runner版本应与Gitlab版本保持同步**
* 可以按需部署多个Gitlab Runner，每个Runner可以运行多个作业
* 类似于Jenkins agent



## 2. 特点

* 作业运行控制：同时执行多个作业。
* 作业运行环境：
    * 在本地、使用Docker容器、使用Docker容器并通过SSH执行作业。
    * 使用Docker容器在不同的云和虚拟化管理程序上自动缩放。
    * 连接到远程SSH服务器。
* 支持Bash, Windows Batch和Windows PowerShell。
* 允许自定义作业运行环境。
* 自动重新加载配置，无需重启。
* 易于安装，可作为Linux、macOS和Windows的服务

## 3. Gitlab Runner类型和特点
* 类型
    * shared共享型，运行整个平台项目的作业(gitlab)
    * group组装型, 运行特定group下的所有项目作业(group)
    * specific专用型, 运行制动的项目作业(project)


* 状态
    * locked: 锁定状态，不能运行作业
    * paused: 暂停状态，暂时不会接收新的作业，比如需要维护时，可以将其置于暂停状态




