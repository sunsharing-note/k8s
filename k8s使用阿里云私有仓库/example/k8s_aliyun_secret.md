## 1、 登录阿里云docker镜像仓库
```
docker login --username=xxx@aliyun.com registry.cn-hangzhou.aliyuncs.com
password:  #输入密码

cat ~/.docker/config.json
# 登录成功后可以通过docker push | docker pull进行上传下载镜像

```

## 2、生成secret
```
kubectl create secret docker-registry regsecret \
--docker-server=registry.cn-hangzhou.aliyuncs.com \
--docker-username=xxx@aliyun.com \
--docker-password=xxxxxx \
--docker-email=xxx@aliyun.com
```

### 解释：
其中:
regsecret: 指定密钥的键名称, 可自行定义
--docker-server: 指定docker仓库地址
--docker-username: 指定docker仓库账号
--docker-password: 指定docker仓库密码
--docker-email: 指定邮件地址(选填)
--namespace=xxx 如果需要指定ns则需要加上这个参数

#### 查看secret
kubectl get secret

## 3、yaml文件加上最重要的两个参数
**imagePullSecrets:**
**- name: regsecret**

```
containers:
- name: channel
  image: registry-internal.cn-hangzhou.aliyuncs.com/yin32167/channel:dev-1.0
ports:
- containerPort: 8114
imagePullSecrets:
- name: regsecret
```

**如果yaml文件中的namespace和创建的secret所在的namespace不同，则kubectl apply -f xxx.yaml以后会发现**
**镜像是下载不下来的**
