**说明**: 尽管ingress可以实现暴露很少的端口，通过域名来提供多种服务，但使用https更为安全，这里将示例tomcat与myapp设置为https访问。

**tomcat**: 假如tomcat通过http方式已经可以访问

* 创建证书：
```
openssl genrsa -out tls.key 2048
openssl req -new -x509 -key tls.key -out tls.crt -subj /CN=tomcat.zaizai.com
```
* 创建secret: ```kubectl create secret tls tomcat-ingress-secret --cert=tls.crt --key=tls.key```
* 编辑ing-tomcat.yaml文件：加上tls,然后apply
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-tomcat
  namespace: default
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  tls:
  - hosts:
    - tomcat.zaizai.com
    secretName: tomcat-ingress-secret
  rules:
  - host: tomcat.zaizai.com
    http:
      paths:
      - path:
        backend:
          serviceName: tomcat
          servicePort: 8080
```
* 最后通过浏览器访问(在本地添加域名解析)

**myapp**: 与tomcat类似，签署证书时，修改CN，创建新的secret,修改ing-myapp.yaml，当然也可以与tomcat的ingress写在一起，这里就没了。
