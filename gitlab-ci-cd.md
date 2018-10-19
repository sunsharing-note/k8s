**参考**：[官网链接](https://docs.gitlab.com/ee/ci/quick_start/ "官网链接")

**说明**：gitlab从8.0开始支持ci,想要使用gitlab ci/cd,则需要先部署gitlab,然后在项目根目录中添加.gitlab-ci.yml,部署runner,runner可以部署在多中环境，
docker、宿主机、kubernetes,最后需要启动runner，注册信息。才能在gitlabUI上选择runner去执行你gitlab-ci.yml中所定义的job。在/etc/gitlab-runner/可查看具体信息。
