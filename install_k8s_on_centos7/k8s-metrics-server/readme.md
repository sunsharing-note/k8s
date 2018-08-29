说明: 用于手机node和pod的cpu和memory使用情况。

可使用metrics-server.yaml文件或者k8s-metrics文件夹中的yaml文件。

有几处需要修改的地方: 一般镜像我们无法直接pull,所以需要自己单独pull，在进行tag。

--source 需要修改


关于角色绑定的需要添加 nodes/stats

另外如果有问题，再把关于minClusterSize的注释掉。