**说明**: 这里是使用NFS来做持久存储,镜像可以用阿里云的

+ 搭建好NFS服务
+ 自己写一个pv pvc来测试NFS可以正常挂载
+ 配置安装nfs-client-provisioner
+ 使用命令查看是否正常
+ 创建pvc验证sc是否能够正常使用
+ 如果使用deployment，则需要先配置pvc，而使用statefuleset则不需要，可直接绑定sc。
+ 以下是配置nfs-client-provisioner的文件

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nfs-client-provisioner
  namespace: kube-system

---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: nfs-client-provisioner-runner
rules:
  - apiGroups: [""]
    resources: ["persistentvolumes"]
    verbs: ["get", "list", "watch", "create", "delete"]
  - apiGroups: [""]
    resources: ["persistentvolumeclaims"]
    verbs: ["get", "list", "watch", "update"]
  - apiGroups: ["storage.k8s.io"]
    resources: ["storageclasses"]
    verbs: ["get", "list", "watch"]
  - apiGroups: [""]
    resources: ["events"]
    verbs: ["list", "watch", "create", "update", "patch"]

---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: run-nfs-client-provisioner
subjects:
  - kind: ServiceAccount
    name: nfs-client-provisioner
    namespace: kube-system 
roleRef:
  kind: ClusterRole
  name: nfs-client-provisioner-runner
  apiGroup: rbac.authorization.k8s.io

---
kind: Deployment
apiVersion: apps/v1beta1
metadata:
  name: nfs-client-provisioner
  namespace: kube-system
spec:
  replicas: 1
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: nfs-client-provisioner
  template:
    metadata:
      labels:
        app: nfs-client-provisioner
    spec:
      serviceAccountName: nfs-client-provisioner
      containers:
        - name: nfs-client-provisioner
          #image: quay.io/external_storage/nfs-client-provisioner:latest
          image: jmgao1983/nfs-client-provisioner:latest
          imagePullPolicy: IfNotPresent
          volumeMounts:
            - name: nfs-client-root
              mountPath: /persistentvolumes
          env:
            - name: PROVISIONER_NAME
              # 此处供应者名字供storageclass调用
              value: nfs-client-provisioner
            - name: NFS_SERVER
              value: 172.17.100.233
            - name: NFS_PATH
              value: /data/logs/nginx
      volumes:
        - name: nfs-client-root
          nfs:
            server: 172.17.100.233
            path: /data/logs/nginx

---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: class-nfs-01      ###pvc或者其他服务绑定动态存储时引用的classname
provisioner: nfs-client-provisioner

````
