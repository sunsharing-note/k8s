全文参考：http://www.cnblogs.com/rongfengliang/p/9661491.html https://github.com/helm/monocular

http://www.cnblogs.com/rongfengliang/p/9653291.html

http://www.cnblogs.com/rongfengliang/p/9661491.html

harbor-helm：https://www.cnblogs.com/rongfengliang/p/9649337.html

**说明**: harbor已经集成helm,可以将helm的包push到Harbor

**先安装helm**: 正常安装 没啥特别的

**安装harbor**: ./install.sh   --with-clair --with-chartmuseum 一定要加后面这两个

```
注意: 需要先在harbor上创建sqi项目
helm repo list
helm repo add --username=admin --password=Harbor12345 sqi https://zhouhua.zaizai.com/chartrepo/sqi
helm repo update
helm repo list
helm create app
helm push -u admin -p Harbor12345 app sqi
```
