```bash
cache:
  paths:
    - target

stages:
  - prepare
  - install
  - deploy_nexus
  - dockerfile
  - deploy_dev
  - deploy_prod
  - deploy_ali_prod

job_prepare:
  stage: prepare
  script:
    - whoami
    - java -version
    - which java
    - mvn -version
    - which mvn
    - mvn clean
    - mvn compile

job_install:
  stage: install
  only:
    - master@g3/TdAppService
  script:
    - mvn -Dmaven.test.skip=true install

job_deploy_nexus:
  stage: deploy_nexus
  only:
    - master@g3/TdAppService
  script:
    - mvn -Dmaven.test.skip=true deploy

job_dockerfile:
  stage: dockerfile
  only:
    - master@g3/TdAppService
  script: 
    - docker build -t dev.app:5000/tdapp/tdappservice .
    - docker push dev.app:5000/tdapp/tdappservice

job_deploy_dev:
  stage: deploy_dev
  when: manual
  only:
    refs:
      - master@g3/TdAppService
    variables:
      - $RELEASE == "dev"
  script:
    - docker service rm tdapp_tdappservice || echo "No such service"
    - docker stack deploy -c docker-stack-tdapp-tdappservice.yml tdapp

job_deploy_prod:
  stage: deploy_prod
  when: manual
  only:
    refs:
      - branches@g3/TdAppService
    variables:
      - $RELEASE == "true"
  script:
    - cat README.md
    - mvn -Dmaven.test.skip=true package
    - docker build -t prod.app:5000/tdapp/tdappservice:$CI_COMMIT_REF_NAME .
    - docker tag prod.app:5000/tdapp/tdappservice:$CI_COMMIT_REF_NAME prod.app:5000/tdapp/tdappservice:latest
    - docker push prod.app:5000/tdapp/tdappservice:$CI_COMMIT_REF_NAME
    - docker push prod.app:5000/tdapp/tdappservice:latest
  environment:
    name: production
    url: http://prod.app:5000/v2/tdapp/tdappservice/tags/list


job_deploy_ali_prod:
  stage: deploy_ali_prod
  when: manual
  only:
    refs:
      - branches@g3/TdAppService
    variables:
      - $RELEASE == "ali"
  script:
    - cat README.md
    - mvn -Dmaven.test.skip=true package
    - docker build -t ali.app:5000/tdapp/tdappservice:$CI_COMMIT_REF_NAME .
    - docker tag ali.app:5000/tdapp/tdappservice:$CI_COMMIT_REF_NAME ali.app:5000/tdapp/tdappservice:latest
    - docker push ali.app:5000/tdapp/tdappservice:$CI_COMMIT_REF_NAME
    - docker push ali.app:5000/tdapp/tdappservice:latest
  environment:
    name: production
    url: http://ali.app:5000/v2/tdapp/tdappservice/tags/list
```

