**说明**: harbor已经集成helm,可以将helm的包push到Harbor

```
**注意**: 需要先在harbor上创建sqi项目
helm repo list
helm repo add --username=admin --password=Harbor12345 sqi https://zhouhua.zaizai.com/chartrepo/sqi
helm repo update
helm repo list
helm create app
helm push -u admin -p Harbor12345 app sqi
```
