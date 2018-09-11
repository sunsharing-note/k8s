参考：https://www.cnblogs.com/heweiblog/p/8931851.html
kubectl  create namespaces dev
echo $(openssl rand -base64 32) > erlang.cookie
kubectl -n dev create secret generic erlang.cookie --from-file=erlang.cookie
自己再创建一个关于密码的secret
