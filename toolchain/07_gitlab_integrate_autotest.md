# Gitlab集成自动化测试

## 1. 开启Gitlab Pages功能

```
vim /etc/gitlab/gitlab.rb

##! Define to enable GitLab Pages
pages_external_url "http://pages.gitlab.com/"
gitlab_pages['enable'] = true
gitlab_pages['inplace_chroot'] = true

# 使配置生效
gitlab-ctl reconfigure
```

## 2. 配置ant job
```
interface_test:
  stage: tests
  tags:
    - build
  script:
    - ant -Djmeter.home=/usr/local/buildtools/apache-jmeter-5.2.1 # 未通过-f指定build.xml文件，默认使用当前目录下的build.xml文件
  artifacts:
    paths:
      - result/htmlfile/
```

## 3. 创建Pages job
```
pages: # 固定关键字
  stage: deploy
  dependencies:
    - interface_test
  script:
    - mv result/htmlfile/ public/
  artifacts:
    paths:
      - public
    expire_in: 30 days
  only:
    - master
```