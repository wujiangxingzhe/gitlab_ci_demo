# installation

## 1. maven是什么
* maven是apache的一个开源项目，是一个项目管理工具，可以对java项目进行构建、依赖管理。
* maven的本质是一个插件，通过插件来完成项目的构建、依赖管理。
* maven的主要功能：
    * 提供了一套标准化的项目结构
    * 提供了一套标准化的构建流程（编译、测试、打包、发布）
    * 提供了一套标准化的依赖管理机制
    * 提供了一套标准化的构建生命周期
    * 提供了一套标准化的构建工具

## 2. maven的安装

### 2.1 安装java
```
sudo yum install -y java-11-openjdk
```

### 2.2 检查java是否安装成功
```
# java -version
openjdk version "11.0.23" 2024-04-16 LTS
OpenJDK Runtime Environment (Red_Hat-11.0.23.0.9-2.el7_9) (build 11.0.23+9-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-11.0.23.0.9-2.el7_9) (build 11.0.23+9-LTS, mixed mode, sharing)
```


### 2.3 下载maven
```
sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.5/binaries/apache-maven-3.9.5-bin.tar.gz
```
* 或者到官网下载最新版本

### 2.4 解压maven
```
sudo tar -zxvf apache-maven-3.9.5-bin.tar.gz -C /opt/
sudo mv /opt/apache-maven-3.9.5 /opt/maven
```


### 2.5 配置maven环境变量
打开 /etc/profile.d/maven.sh 文件（如果不存在则创建）
```
sudo vim /etc/profile.d/maven.sh
```

添加以下内容：
```
export M2_HOME=/opt/maven
export PATH=$M2_HOME/bin:$PATH
```

加载环境变量
```
source /etc/profile.d/maven.sh
```

### 2.6 检查maven是否安装成功
```
# mvn -version
Apache Maven 3.9.5 (57804ffe001d7215b5e7bcb531cf83df38f93546)
Maven home: /opt/maven
Java version: 11.0.23, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-11-openjdk-11.0.23.0.9-2.el7_9.x86_64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1160.119.1.el7.x86_64", arch: "amd64", family: "unix"
```

### 2.7 配置maven仓库
Maven 的配置文件位于 conf/settings.xml，通常在 /opt/maven/conf/settings.xml



### 2.8 配置maven
运行以下命令创建一个简单的 Maven 项目
```
# mvn archetype:generate -DgroupId=com.example -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
...
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: basedir, Value: /opt/app/maven
[INFO] Parameter: package, Value: com.example
[INFO] Parameter: groupId, Value: com.example
[INFO] Parameter: artifactId, Value: my-app
[INFO] Parameter: packageName, Value: com.example
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: /opt/app/maven/my-app
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  40.376 s
[INFO] Finished at: 2025-01-22T05:11:54-05:00
[INFO] ------------------------------------------------------------------------

# ls
my-app
# tree my-app/
my-app/
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── example
    │               └── App.java
    └── test
        └── java
            └── com
                └── example
                    └── AppTest.java

9 directories, 3 files
```

