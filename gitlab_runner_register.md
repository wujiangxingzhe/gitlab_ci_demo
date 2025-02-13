# 注册Gitlab Runner


## 1. 注册
* 获取token，在创建instance runner的时候，会生成注册命令，注册命令中包含token， gitlab-runner register  --url http://192.168.50.130:9000  --token glrt-t1_hHHu2AHsNgPKE-Na9ddr
* 注册

```
[root@cent7 packages]# gitlab-runner register  --url http://192.168.50.130:9000  --token glrt-t1_hHHu2AHsNgPKE-Na9ddr
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


## 2. 配置


