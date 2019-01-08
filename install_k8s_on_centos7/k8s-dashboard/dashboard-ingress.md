* 说明：[参考地址](https://note.youdao.com/ynoteshare/index.html?id=7e46114e9f29d022f09bff685261f534&type=note#/ "参考地址")
* 通过ingress集成dashboard
* 注意：ingress-controller必须使用0.21.0以上
* 测试过程中问题：登录时，直接跳过则无权限访问 通过token登录 点击登陆时没反应 这里我通过将kubernetes-dashboard绑定了clusterrole为
* cluster-admin 直接跳过就可以访问 但不安全

```
1、修改Kubernetes-dashboard.yaml
vim kubernetes-dashboard.yaml 主要修改以下带####
        ports:
        - containerPort: 9090  ####
          protocol: TCP
        args:
          - --insecure-port=9090 ####
          - --enable-insecure-login=true ####
          - --authentication-mode=token ####
        volumeMounts:
        - name: kubernetes-dashboard-certs
          mountPath: /certs
        - mountPath: /tmp
          name: tmp-volume
        livenessProbe:
          httpGet:
            scheme: HTTP
            path: /
            port: 9090 #####
---------------------------------
spec:
  ports:
    - port: 9090   ####
      targetPort: 9090  ####
      nodePort: 30000  ####
  selector:
    k8s-app: kubernetes-dashboard
  type: NodePort #####

2、修改ingress-dashboard.yaml
vim ingress-dashboard.yaml
apiVersion: v1
kind: List
items:
- apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    name: k8s-dashboard
    namespace: kube-system
    annotations:
      nginx.ingress.kubernetes.io/ssl-redirect: "true"
      nginx.ingress.kubernetes.io/rewrite-target: /
      nginx.ingress.kubernetes.io/secure-backends: "true"
  spec:
    tls:
    - hosts:
      - tomcat.zaizai.com ####
      secretName: k8s-dashboard-secret ####
    rules:
    - host: tomcat.zaizai.com  ####
      http:
        paths:
        - path: /
          backend:
            serviceName: kubernetes-dashboard
            servicePort: 9090   ####

3、登录测试即可
```
