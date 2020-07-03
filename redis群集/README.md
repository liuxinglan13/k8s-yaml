### K8s中部署Redis群集
参考地址:https://blog.csdn.net/zhutongcloud/article/details/90768390
#### 使用方法
* 所有的资源全部创建至namespace redis
* 创建configmap
```
kubectl create configmap redis-conf --from-file=redis.conf
```
* 创建Deployment,service
```
kubectl create -f redis.yaml
```