# Gitlab CICD自动发布流程

通过trigger触发下游

## 1. 定义接口测试job模板
```
- .interfacetest:
    stage: interface_test
    trigger:
      project: cidevops/cidevops-interfacetest-service
      branch: master
      strategy: depend
```

## 2. 在pipeline模板中添加接口测试job模板

