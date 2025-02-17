# docker image integration

```
.build-docker:
  stage: buildimage
  tags:
    - build
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t ${IMAGE_NAME} -f ${DOCKER_FILE_PATH} .
    - docker push ${IMAGE_NAME}
    - docker rmi ${IMAGE_NAME}
```
* 变量定义
```
  # 构建镜像
  CI_REGISTRY: 'registry.cn-beijing.aliyuncs.com'
  CI_REGISTRY_USER: '610556220zy'
  #CI_REGISTRY_PASSWD: 'xxxxxxxx' # 通过Gitlab UI进行定义
  IMAGE_NAME: "$CI_REGISTRY/$CI_PROJECT_NAME:$CI_COMMIT_REF_NAME-$CI_COMMIT_SHORT_SHA-$CI_PIPELINE_ID"
  DOCKER_FILE_PATH: "./Dockerfile"
```
