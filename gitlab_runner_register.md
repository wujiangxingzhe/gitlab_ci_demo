# 注册Gitlab Runner


## 1. 注册
* 获取token，在创建instance runner的时候，会生成注册命令，注册命令中包含token， gitlab-runner register  --url http://192.168.50.130:9000  --token glrt-t1_hHHu2AHsNgPKE-Na9ddr
* 注册shell和docker executor不能使用相同的token，需要重新获取token
### 1.1  注册 shell executor
```
[root@cent7 packages]# gitlab-runner register --url http://192.168.50.130:9000  --token glrt-t1_hHHu2AHsNgPKE-Na9ddr --executor shell
Runtime platform                                    arch=amd64 os=linux pid=13268 revision=3153ccc6 version=17.7.0
Running in system-mode.                            
                                                   
Enter the GitLab instance URL (for example, https://gitlab.com/):
[http://192.168.50.130:9000]: 
Verifying runner... is valid                        runner=t1_hHHu2A
Enter a name for the runner. This is stored only in the local config.toml file:
[cent7]: host-instance-runner
Enter an executor: custom, docker-autoscaler, parallels, virtualbox, docker, docker-windows, docker+machine, kubernetes, shell, ssh, instance:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
 
Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml" 
```

### 1.2 注册 docker executor
```
gitlab-runner register \
  --non-interactive \
  --executor "docker" \
  --docker-image alpine:latest \
  --url "http://192.168.50.130:9000" \
  --registration-token "glrt-t1_z4Hvhr9NKDzhkjndFnVx" \
  --description "docker-runner" \
  --tag-list "docker,aws" \
  --run-untagged="true" \
  --locked="false" \
  --access-level="not_protected"
```
* 最佳实践是设置docker image的策略为`if-not-present`, 即`pull_policy="if-not-present"`
* 在使用`services`关键字时，如果没有指定`image`，则默认使用`docker`的`image`

## 2. 配置


