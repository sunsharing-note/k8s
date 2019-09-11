
## k8s使用阿里云镜像仓库注意点

```
secret所在namespace必须和deployment或者RC创建出来的pod在同一个ns。
否则apply生成pod的时候会发现拉取镜像失败。describe发现提示没权限。
```
