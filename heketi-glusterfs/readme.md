### 在kubernetes中部署Heketi和GlusterFS配置动态pv

**为什么要使用heketi?**

因为GlusterFS自身并不具备RESTful API，而k8s必须通过Restful API请求来创建pv。
Kubernetes可以通过Heketi管理GlusterFS卷的生命周期的，Heketi为GlusterFS提供了一个RESTful API 接口供Kubernetes调用，通过Heketi，Kubernetes可以动态配置GlusterFS卷，Heketi会动态在集群内选择bricks创建所需的volumes，确保数据的副本会分散到集群不同的故障域内，同时Heketi还支持GlusterFS多集群管理，便于管理员对GlusterFS进行操作。

服务器|ip|存储ip|角色
-|-|-|-
node1|xxxxx|xxxxx|K8s Node+GlusterFS Node
node2|xxxxx|xxxxx|K8s Node+GlusterFS Node
node3|xxxxx|xxxxx|K8s Node+GlusterFS Node

##### 部署步骤

1.安装heketi客户端工具，[heketi下载地址](https://github.com/heketi/heketi/releases)

2.在集群内部署glusterfs-server,参考文件：deploy-glusterfs.json.

&emsp;提前给3个node打标签，即kubectl label node xxx.xxx.xxx.xx storagenode=glusterfs

&emsp;部署： kubectl create -f glusterfs-daemonset.json

&emsp;验证： kubectl get pod # pod必须running且ready

3.在集群内部署heketi服务端,参考文件：heketi-bootstrap.json

&emsp;先创建相关账户，参考：heketi-service-account.json：kubectl create -f heketi-service-account.json

&emsp;为账户创建角色绑定：kubectl create clusterrolebinding heketi-gluster-admin --clusterrole=edit --serviceaccount=default:heketi-service-account

&emsp;创建secret来保存heketi服务的配置，参考:heketi.json：kubectl create secret generic heketi-config-secret --from-file=./heketi.json

&emsp;部署：kubectl create -f heketi-bootstrap.json

&emsp;验证：kubeget get pod # 须为running且ready

4.测试heketi服务端

&emsp;kubectl port-forward  deploy-heketi-8465f8ff78-sb8z 8080:8080

&emsp;curl http://localhost:8080/hello
   Handling connection for 8080
   Hello from heketi

5.heketi管理glusterfs-server,通过HEKETI_CLI_SERVER来设置heketi服务端

&emsp;export HEKETI_CLI_SERVER=http://10.254.238.186:8080 # service的ip，我这里是

&emsp;说明：拓朴文件，它提供了运行gluster Pod的kubernetes节点IP，每个节点上相应的磁盘块设备,参考：topology.json

&emsp;加载拓扑文件：heketi-cli topology load --json=topology.json

&emsp;查看结果：heketi-cli topology info

6.创建storageclass,参考：storageClass.yaml

&emsp;应用：kubectl apply -f storageClass.yaml

7.创建pvc进行测试，参考test-pvc.yaml

&emsp;应用：kubectl apply -f test-pvc.yaml

&emsp;查看：kubectl get pvc -n router


8.注意事项

&emsp;错误提示：unknown filesystem "glusterfs"，解决：在glusterfs节点安装：yum -y install glusterfs-fuse

&emsp;错误提示：thin: Required device-mapper target(s) not detected in your kernel.，解决：加载模块dm_thin_pool,用modprobe dm_thin_pool

&emsp;错误提示：failed to create volume: failed to create volume: sed: can't read /var/lib/heketi/fstab: No such file or directory,解决：创建该文件





