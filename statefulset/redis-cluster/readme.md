##### redis-cluster: [redis-cluster参考链接](https://www.jianshu.com/p/65c4baadf5d9 "redis-cluster参考链接")
1. 搭建nfs
2. 测试nfs能否正常挂载
3. 利用nfs创建pv
4. 创建redis.conf配置文件，并创建configmap
5. 创建redis headless service
6. 创建6个redispod 用statefulset  有状态
7. 在创建一个centos pod，并进入其中利用redis-trib.rb配置redis-cluster,利用dig DNS
8. 测试redis-cluster，如果主从不对或有主无从，可使用cluster replicate master_node_id修改
9. 为供外界访问，创建redis-service，拥有clusterIP，type可以采用nodeport
10. 具体程序环境测试。
