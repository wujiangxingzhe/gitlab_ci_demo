# 代码质量工具——SonarQube集成

## 1. 环境准备
* 安装SonarQube，客户端sonar-scanner
* 配置codeanalysis.yml


### 1.1 codeanalysis.yml
```
.codeanalysis-java:
  stage: code_analysis
  tags:
    - build
  script:
    - echo $CI_MERGE_REQUEST_IID $CI_MERGE_REQUEST_SOURCE_BRANCH_NAME $CI_MERGE_REQUEST_TARGET_BRANCH_NAME
    - "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=${CI_PROJECT_NAME} \
      -Dsonar.projectName=${CI_PROJECT_NAME} \
      -Dsonar.projectVersion=${CI_COMMIT_REF_NAME} \
      -Dsonar.ws.timeout=30 \
      -Dsonar.projectDescription=${CI_PROJECT_TITLE} \
      -Dsonar.links.homepage=${CI_PROJECT_URL} \
      -Dsonar.sources=${SCAN_DIR} \
      -Dsonar.sourceEncoding=UTF-8 \
      -Dsonar.java.binaries=target/classes \
      -Dsonar.java.test.binaries=target/test-classes \
      -Dsonar.java.surefire.report=target/surefire-reports \
      -Dsonar.branch.name=${CI_COMMIT_REF_NAME}" # 支持多分支，需要安装插件支持
  artifacts:
    paths:
      - "$ARTIFACT_PATH"
```