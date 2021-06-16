<h2>k8s上搭建redis(哨兵模式)</h2>

#### [Refer](https://github.com/bitnami/charts/blob/master/bitnami/redis/README.md)


```js
//add package
1: helm repo add bitnami https://charts.bitnami.com/bitnami

// install redis by params
2: helm install redis bitnami/redis /
--set replica.replicaCount=4 /
--set master.persistence.enabled=false /
--set metrics.enabled=false /
--set sentinel.enabled=true /
--set auth.enable=false /
--set auth.sentinel=false
```

#### k8s 集群输出：
```js
NAME: redis
LAST DEPLOYED: Wed Jun 16 15:46:07 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

Redis(TM) can be accessed via port 6379 on the following DNS name from within your cluster:

    redis.default.svc.cluster.local for read only operations

For read/write operations, first access the Redis(TM) Sentinel cluster, which is available in port 26379 using the same domain name above.



To connect to your Redis(TM) server:

1. Run a Redis(TM) pod that you can use as a client:

   kubectl run --namespace default redis-client --restart='Never'  --image docker.io/bitnami/redis:6.2.4-debian-10-r0 --command -- sleep infinity

   Use the following command to attach to the pod:

   kubectl exec --tty -i redis-client \
   --namespace default -- bash

2. Connect using the Redis(TM) CLI:
   redis-cli -h redis -p 6379 # Read only operations
   redis-cli -h redis -p 26379 # Sentinel access

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace default svc/redis 6379:6379 &
    redis-cli -h 127.0.0.1 -p 6379
```

结果:
```js
$ kl get pods
NAME           READY   STATUS    RESTARTS   AGE
redis-node-0   2/2     Running   0          3m11s
redis-node-1   2/2     Running   0          2m53s
redis-node-2   2/2     Running   0          2m36s
redis-node-3   2/2     Running   0          2m11s
```



