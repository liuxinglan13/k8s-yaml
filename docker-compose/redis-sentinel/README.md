###Docker-compose部署Redis 1主2从 哨兵群集

参考:https://juejin.im/post/5d07ac98e51d4577583ddccc

参考:https://www.cnblogs.com/kismetv/p/9609938.html#t3

* 单主机部署

文件结构:

Redis-sentinel

├── data1
│   
├── data2
│   
├── data3
│   
├── docker-compose.yaml
├── sentinel1.conf
├── sentinel2.conf
└── sentinel3.conf

#### 端口分配

| redis-master     | 16379 |
| ---------------- | ----- |
| redis-slave-1    | 16380 |
| redis-slave-2    | 16381 |
| redis-sentinel-1 | 26379 |
| redis-sentinel-2 | 26380 |
| redis-sentinel-3 | 26381 |



#### 部署

```
docker-compose up -d
```

#### 验证

* 主从同步验证及数据持久化严重

```
#从外部主机连接
redis-cli -h IP -p 16379
#进行set操作,再连接其余两节点get观察数据是否存在
#docker-compose down 删除所有容器之后再 docker-compose up -d
#再次连接redis观察之前set的数据是否还存在
```

* 验证哨兵状态

```
#连接哨兵端口
redis-cli -h IP -p 26379
#常用命令
info sentinel：获取监控的所有主节点的基本信息
sentinel masters：获取监控的所有主节点的详细信息
sentinel master mymaster：获取监控的主节点mymaster的详细信息
sentinel slaves mymaster：获取监控的主节点mymaster的从节点的详细信息
sentinel sentinels mymaster：获取监控的主节点mymaster的哨兵节点的详细信息
sentinel get-master-addr-by-name mymaster：获取监控的主节点mymaster的地址信息，前文已有介绍
sentinel is-master-down-by-addr：哨兵节点之间可以通过该命令询问主节点是否下线，从而对是否客观下线做出判断
```

* 测试节点故障转移

```
#首先获取主节点信息
sentinel get-master-addr-by-name mymaster
172.16.150.70:26380> sentinel get-master-addr-by-name mymaster
1) "172.16.150.70"
2) "16380"
#将主节点所在容器stop,等待10秒
172.16.150.70:26380> sentinel get-master-addr-by-name mymaster
1) "172.16.150.70"
2) "16379"
#主节点已切换,连接新的主节点进行set操作
redis-cli -h 172.16.150.70 -p 16379
172.16.150.70:16379> set g 1
OK

```

