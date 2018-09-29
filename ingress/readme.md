ingress具体使用参考：
+ 1、https://github.com/kubernetes/ingress-nginx/tree/master/deploy
+ 2、https://www.cnblogs.com/justmine/p/8991379.html
                    
                    
在最新的ingress部署中，在with-rbac.yaml文件中缺少service-nodeport.yaml中需要选择的标签，需要手动加上。
