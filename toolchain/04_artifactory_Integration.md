# Artifactory Integration

整体的思路，是通过将Artifactory API以command的方式集成到Gitlab CI的job中，实现制品的自动上传
* 上传制品
```
.deploy-artifact:
  stage: deploy-artifact
  tags:
    - build
  script:
    - curl -u${ARTIFACT_USER}:${ARTIFACT_PASSWD} -T ${ARTIFACT_PATH} "${ARTIFACTORY_URL}/${ARTIFACTORY_NAME}/${TARGET_FILE_PATH}/${TARGET_ARTIFACT_NAME}"
```

* 下载制品
```
.download-artifact:
  stage: download-artifact
  tags:
    - build
  script:
    - curl -u${ARTIFACT_USER}:${ARTIFACT_PASSWD} -O "${ARTIFACTORY_URL}/${ARTIFACTORY_NAME}/${TARGET_FILE_PATH}/${TARGET_ARTIFACT_NAME}"
    - ls
```

* 变量定义
  * 敏感信息通过Gitlab UI进行管理，比如username和password
```
  ARTIFACT_PATH: 'target/*.jar'  ##制品目录

  # 上传制品库
  ARTIFACTORY_URL: "http://192.168.1.200:30082/artifactory"
  ARTIFACTORY_NAME: "cidevops"
  TARGET_FILE_PATH: "$CI_PROJECT_NAMESPACE/$CI_PROJECT_NAME/$CI_COMMIT_REF_NAME-$CI_COMMIT_SHORT_SHA-$CI_PIPELINE_ID"
  TARGET_ARTIFACT_NAME: "$CI_PROJECT_NAME-$CI_COMMIT_REF_NAME-$CI_COMMIT_SHORT_SHA-$CI_PIPELINE_ID.jar"
```

