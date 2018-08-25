# dashboard的说明

1、创建时使用的文件kubernetes-dashboard.yml或者另外一个1.8.3的，需要修改里面的镜像文件，我们最好提前pull镜像，使用docker tag标签为文件需要的，当然
也可以根据自己的喜好进行修改。
2、访问地址为：https://{YOUR_VIP}:6443/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/其中修改为你的服务器实际IP与端口即可。
1.7之后需要提供Kubeconfig后者token进行访问。

 2.1 token方式
   SECRET=$(kubectl -n kube-system get sa kubernetes-dashboard -o yaml | awk '/dashboard-token/ {print $3}')
   kubectl -n kube-system describe secrets ${SECRET} | awk '/token:/{print $2}'
   将token复制粘贴即可进入界面，如进入界面后提示权限问题，则需要执行以下命令
   kubectl create clusterrolebinding kubernetes-dashboard --clusterrole cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
   具体名字视具体情况而定
 2.2 kubeconfig
  参考：access_by_token_and_kubeconfig
