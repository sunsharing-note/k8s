**说明**: dashboard限制权限访问 首先需要输入用户名和密码，其次需要token，不能直接跳过

* 部署dashboard相关 kubernetes-dashboard.yaml ui-ceshi-rbac.yaml /etc/kubernetes/ssl/basic-auth.csv 

* 创建sa admin以及clusterrolebinding

```
kubectl create sa admin -n kube-system
kubectl create clusterrolebinding admin --clusterrole=cluster-admin --serviceaccount=kube-system:admin
最后获取admin的token进行访问即可
```
