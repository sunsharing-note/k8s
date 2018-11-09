# 升级harbor:https://github.com/goharbor/harbor/blob/master/docs/migration_guide.md
**删除harbor中的镜像**：[参考链接](https://blog.csdn.net/felix_yujing/article/details/78626907?locationNum=3&fps=1 "参考链接")

**说明**: 之前没有对docker push镜像做过多的控制和要求，导致现在harbor中镜像占用很大空间，而一些时间久远不再需要的镜像可清除，目前采用以下方式处理：

* 在harborUI界面，手工删除不再需要的镜像
* #目前采用手工，有api，后续看利用api或者Python写脚本去处理
* docker-compose stop
* docker run -it --name gc --rm --volumes-from registry vmware/registry:2.6.2-photon garbage-collect --dry-run /etc/registry/config.yml
* docker run -it --name gc --rm --volumes-from registry vmware/registry:2.6.2-photon garbage-collect /etc/registry/config.yml
* docker-compose start
