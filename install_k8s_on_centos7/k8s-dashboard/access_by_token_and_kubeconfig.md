+ 我们在配置好了dashboard之后，也就是执行了$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
+ 通过https://{YOUR_VIP}:6443/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/访问dashboard时，可能会出现无法访问或者需要通过token或者kubeconfig去访问。这里来说一下如何去获得token或者Kubeconfig

**注**: 访问dashboard时，提示message": "services \"https:kubernetes-dashboard:\" is forbidden: User \"system:anonymous\" cannot get services/proxy in the namespace \"kube-system\"",
说明该用户无权访问，我们想要访问所有的ns里面的东西，所以给该用户绑定一个clusterrole，执行anonymous-proxy-rbac.yml该文件。再去访问即可。

**注**：因为我们希望通过dashboard去帮我们管理整个集群，所以需要通过serviceaccount，并且需要具有clusterrole相关权限。提供的证书中也必须是serviceaccount。

1、token

通过
+ SECRET=$(kubectl -n kube-system get sa kubernetes-dashboard -o yaml | awk '/dashboard-token/ {print $3}') 获取token
+ kubectl -n kube-system describe secrets ${SECRET} | awk '/token:/{print $2}' 即可获取到token,粘贴即可访问。

**自定义创建用户** 能够看到所有ns下的资源等
+ kubectl create sa huahua -n kube-system  创建serviceaccount
+ kubectl create clusterrolebinding huahua --clusterrole=cluster-admin --serviceaccount=kube-system:huahua 创建clusterrolebinding
+ SECRET=$(kubectl -n kube-system get sa huahua -o yaml | awk '/dashboard-token/ {print $3}') 获取secret
+ kubectl -n kube-system describe secrets ${SECRET} | awk '/token:/{print $2}' 获取token进行访问

**假如只希望一个用户管理自己所在ns中的资源，那就用rolebinding绑定**

+ kubectl create sa huahua -n default 创建用户
+ kubectl create rolebinding huahua --clusterrole=cluster-admin --serviceaccount=default:huahua 创建rolebinding
+ SECRET=$(kubectl -n kube-system get sa huahua -o yaml | awk '/dashboard-token/ {print $3}') 获取secret
+ kubectl -n kube-system describe secrets ${SECRET} | awk '/token:/{print $2}' 获取token进行访问

**注**：为什么是用clusterrole呢？ 因为rolebinding可以绑定clusterrole，这样子sa就只具有集群中自己所在的ns中的资源的相关权限。

2、kubeconfig

+ kubectl create sa huahua -n default 创建用户
+ kubectl create rolebinding huahua --clusterrole=admin --serviceaccount=default:huahua -n default 创建rolebinding
+ ###SECRET=$(kubectl -n kube-system get sa huahua -o yaml | awk '/dashboard-token/ {print $3}')
+ ###kubectl -n kube-system describe secrets ${SECRET} | awk '/token:/{print $2}'
+ 通过sa huahua得到secret,再通过secret得到token：kubectl get secret huahua-token-xqsbb -o yaml
+ 解码token：TOKEN=$(echo token | base64 -d)
+ 查看当前config：kubectl config view
+ 设置集群：kubectl config set-cluster kubernetes --certificate-authority=/etc/kubernetes/pki/ca.crt --server="https://192.168.40.200:6443" --embed-certs=true --kubeconfig=/data/kubernetes.conf
+ 设置user：kubectl config set-credentials huahua --token=$TOKEN --kubeconfig=/data/kubernetes.conf
+ 设置context: kubectl config set-context huahua@kubernetes --cluster=kubernetes --user=huahua --kubeconfig=/data/kubernetes.conf
+ 设置当前context: kubectl config use-context huahua@kubernetes --kubeconfig=/data/kubernetes.conf
再将/data/kubernetes.conf放到需要访问dashboard的宿主机，引用即可访问。huahua属于哪个ns,你就只可以访问该ns相关资源。

**注**：这里是通过token去做的，也可以通过证书去做，使用openssl。



