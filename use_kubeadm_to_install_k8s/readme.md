之前都是通过下载二进制包之后，手动安装的k8s，过程及其繁琐，花费时间和精力。因此，现使用官方推荐kubeadm进行安装，快捷，方便。个人认为了解了环境的整个架构即可，没必要在搭建环境上花费太多时间，更重要的是能够解决实际问题。

本次安装参考或使用到的网站：1、官网
                          2、https://github.com/coreos/flannel
                          3、https://hub.docker.com/
                          4、https://github.com/kubernetes/kubernetes
                          5、https://opsx.alibaba.com/mirror #阿里巴巴镜像网站
 另外，特别重要的是，由于一些不可描述的原因，整个过程中使用到的镜像，我们无法直接pull下来，我这里是从别人那里pull下来之后，push到了自己的dockerhub上，方便以后使用。无论后续是否需要，建议将使用到的镜像save，或者push到自己的dockerhub上，以方便使用。
 
 
 由于是搭建完成之后，再进行的笔记记录，可能不够详尽，但一般问题都不大。可自行解决。
在10-kubeadm.conf中添加这一行，通过阿里云去转发

Environment="KUBELET_EXTRA_ARGS=--v=2 --fail-swap-on=false --pod-infra-container-image=registry.cn-hangzhou.aliyuncs.com/k8sth/pause-amd64:3.0"
