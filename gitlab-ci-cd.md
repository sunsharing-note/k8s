**参考**：[官网链接](https://docs.gitlab.com/ee/ci/quick_start/ "官网链接")

**说明**：gitlab从8.0开始支持ci,想要使用gitlab ci/cd,则需要先部署gitlab,然后在项目根目录中添加.gitlab-ci.yml,部署runner,runner可以部署在多中环境，docker、宿主机、kubernetes,最后需要启动runner，注册信息。才能在gitlabUI上选择runner去执行你gitlab-ci.yml中所定义的job。在/etc/gitlab-runner/可查看具体信息。

**.gitlab-ci.yml**

* 文件中定义了job，runner负责去跑其定义的job，一旦有代码提交，且runner启动了，则就会自动去跑job。通过API与gitlab通信。
* stages:默认有build、test、deploy，可自行定义
* stage:如果未定义，则默认为test
* script:具体执行的命令


```
stages:
  - build
  - test

test1:
  stage: build
  tags:
  - k8s-master-shell
  image: centos:latest
  script:
  - source /etc/profile
  - pwd
```
