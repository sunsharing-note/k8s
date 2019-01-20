过程中若有镜像无法直接下载，请到dockerhub上查找pull再进行tag: [传送至dockerhub官网](https://hub.docker.com/ "传送至dockerhub")

ansible一键安装：[ansible安装k8s](https://github.com/gjmzj/kubeasz "传送")

- 小米正式开源Istio管理面板Naftis is http://www.servicemesher.com/blog/naftis-istio-dashboard-xiaomi/
- istio-ui——一款开源的简易Istio UI的介绍和使用教程 教程 http://www.servicemesher.com/blog/istio-ui-tutorial/
- Kiali——Istio Service Mesh 的可观察性工具 工具 http://www.servicemesher.com/blog/kiali-the-istio-service-mesh-observability-tool/
- Istio1.1.0下的TCP流量控制 控制 http://www.servicemesher.com/blog/raw-tcp-traffic-shaping-with-istio/
- 服务网格是中间件的终结者吗？者吗？http://www.servicemesher.com/blog/does-the-service-mesh-spell-the-end-for-middleware/

- calico开启IPIP模式，实现跨网段通信。参考: [参考链接](https://www.lijiaocn.com/%E9%A1%B9%E7%9B%AE/2017/09/25/calico-ipip.html "参考链接")

```
k8s的1.12版本镜像：
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/k8s.1-12-1.tar.gz
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/k8s.1-12-4.tar.gz

k8s的1.13版本镜像：
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/k8s.1-13-1.tar.gz
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/k8s.1-13-2.tar.gz

https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/basic_images_kubeasz_0.4.tar.gz

harbor 1.6.1下载：
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/harbor-offline-installer-v1.6.1.tgz

harbor 1.7.0下载：
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/harbor-offline-installer-v1.7.0.tgz


harbor 1.7.1下载：
https://qjdstatic.oss-cn-hangzhou.aliyuncs.com/clms1/1499762586142/static/dtalk/k8s/harbor-offline-installer-v1.7.1.tgz
```

**创建证书**:

```
openssl x509 -req -days 3650 -in zhouhua.zaizai.com.csr -signkey zhouhua.zaizai.com.key -out zhouhua.zaizai.com.crt 

openssl x509 -req -in zhouhua.zaizai.com.csr -CA zhouhua.zaizai.com.crt -CAkey zhouhua.zaizai.com.key -CAcreateserial -out zhouhua.zaizai.com.crt -days 10000 

openssl req -newkey rsa:2048 -nodes -sha256 -keyout /data/cert/zhouhua.zaizai.com.key -x509 -days 365 -out /data/cert/zhouhua.zaizai.com.crt 
```
