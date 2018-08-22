Getting Started with Knative App Deployment
======

## 部署应用

```
$ k create -f service.yaml
```

```
$ k get services.serving.knative.dev
NAME            CREATED AT
helloworld-go   2m
```

## 访问应用

1. 获取Ingress地址

```
$ kubectl get svc knative-ingressgateway -n istio-system
NAME                     TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                                      AGE
knative-ingressgateway   LoadBalancer   172.19.14.176   47.95.181.182   80:32380/TCP,443:32390/TCP,32400:32400/TCP   10m
```

或者

```
kubectl get svc knative-ingressgateway -n istio-system -o 'jsonpath={.status.loadBalancer.ingress[0].ip}'
47.95.181.182
```

```
export IP_ADDRESS=$(kubectl get svc knative-ingressgateway -n istio-system -o 'jsonpath={.status.loadBalancer.ingress[0].ip}')
```

2. 获取应用的域名

```
kubectl get services.serving.knative.dev helloworld-go  -o=custom-columns=NAME:.metadata.name,DOMAIN:.status.domain
```

```
export HOST_URL=$(kubectl get services.serving.knative.dev helloworld-go  -o jsonpath='{.status.domain}')
```

3. 访问应用

```
$ curl -H "Host: ${HOST_URL}" http://$IP_ADDRESS
Hello World: Go Sample v1!
```

## 总结

* Service:自动管理workload的整个生命周期,它控制其他对象的创建，以确保应用程序具有每次服务更新的Route，Configuration, Revision。Service可以定义将为将流量路由到最新版本或者固定版本

```
$ k get service.serving.knative.dev helloworld-go -o yaml
```

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  clusterName: ""
  creationTimestamp: 2018-08-22T08:53:22Z
  generation: 1
  name: helloworld-go
  namespace: default
  resourceVersion: "20884602"
  selfLink: /apis/serving.knative.dev/v1alpha1/namespaces/default/services/helloworld-go
  uid: d2cdfe58-a5e8-11e8-9193-00163e1014f3
spec:
  generation: 2
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          creationTimestamp: null
        spec:
          concurrencyModel: Multi
          container:
            env:
            - name: TARGET
              value: Go Sample v2
            image: registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/helloworld-go
            name: ""
            resources: {}
status:
  conditions:
  - lastTransitionTime: 2018-08-22T08:53:58Z
    status: "True"
    type: RoutesReady
  - lastTransitionTime: 2018-08-22T09:00:00Z
    status: "True"
    type: ConfigurationsReady
  - lastTransitionTime: 2018-08-22T09:00:00Z
    status: "True"
    type: Ready
  domain: helloworld-go.default.example.com
  domainInternal: helloworld-go.default.svc.cluster.local
  latestCreatedRevisionName: helloworld-go-00004
  latestReadyRevisionName: helloworld-go-00004
  observedGeneration: 2
  traffic:
  - configurationName: helloworld-go
    percent: 100
    revisionName: helloworld-go-00004
```

* Route: 负责将网络端点映射到一个或多个Revision，用户可以通过百分比和命名路由的方式管理流量

```
k get route.serving.knative.dev helloworld-go -o yaml
```

```
apiVersion: serving.knative.dev/v1alpha1
kind: Route
metadata:
  clusterName: ""
  creationTimestamp: 2018-08-22T08:53:22Z
  generation: 1
  labels:
    serving.knative.dev/service: helloworld-go
  name: helloworld-go
  namespace: default
  ownerReferences:
  - apiVersion: serving.knative.dev/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: Service
    name: helloworld-go
    uid: d2cdfe58-a5e8-11e8-9193-00163e1014f3
  resourceVersion: "20884599"
  selfLink: /apis/serving.knative.dev/v1alpha1/namespaces/default/routes/helloworld-go
  uid: d2d3f9d5-a5e8-11e8-b315-00163e12479f
spec:
  generation: 1
  traffic:
  - configurationName: helloworld-go
    percent: 100
status:
  conditions:
  - lastTransitionTime: 2018-08-22T08:53:58Z
    status: "True"
    type: AllTrafficAssigned
  - lastTransitionTime: 2018-08-22T08:53:58Z
    status: "True"
    type: Ready
  domain: helloworld-go.default.example.com
  domainInternal: helloworld-go.default.svc.cluster.local
  traffic:
  - configurationName: helloworld-go
    percent: 100
    revisionName: helloworld-go-00004
```

* Configuration：维护Deployment到所需的状态

```
apiVersion: serving.knative.dev/v1alpha1
kind: Configuration
metadata:
  clusterName: ""
  creationTimestamp: 2018-08-22T08:53:22Z
  generation: 1
  labels:
    serving.knative.dev/route: helloworld-go
    serving.knative.dev/service: helloworld-go
  name: helloworld-go
  namespace: default
  ownerReferences:
  - apiVersion: serving.knative.dev/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: Service
    name: helloworld-go
    uid: d2cdfe58-a5e8-11e8-9193-00163e1014f3
  resourceVersion: "20884591"
  selfLink: /apis/serving.knative.dev/v1alpha1/namespaces/default/configurations/helloworld-go
  uid: d2d2a03c-a5e8-11e8-b315-00163e12479f
spec:
  generation: 4
  revisionTemplate:
    metadata:
      creationTimestamp: null
    spec:
      concurrencyModel: Multi
      container:
        env:
        - name: TARGET
          value: Go Sample v2
        image: registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/helloworld-go
        name: ""
        resources: {}
status:
  conditions:
  - lastTransitionTime: 2018-08-22T09:00:00Z
    status: "True"
    type: Ready
  latestCreatedRevisionName: helloworld-go-00004
  latestReadyRevisionName: helloworld-go-00004
  observedGeneration: 4
```

* Revision:对workload进行的每个修改的代码和配置的时间点快照。Revision是不可变对象，只要有用就可以保留

```
$ k get revision.serving.knative.dev
helloworld-go-00001   16m
helloworld-go-00002   11m
helloworld-go-00003   11m
helloworld-go-00004   9m
```

```
apiVersion: serving.knative.dev/v1alpha1
kind: Revision
metadata:
  annotations:
    serving.knative.dev/configurationGeneration: "4"
  clusterName: ""
  creationTimestamp: 2018-08-22T08:59:52Z
  generation: 1
  labels:
    serving.knative.dev/configuration: helloworld-go
  name: helloworld-go-00004
  namespace: default
  ownerReferences:
  - apiVersion: serving.knative.dev/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: Configuration
    name: helloworld-go
    uid: d2d2a03c-a5e8-11e8-b315-00163e12479f
  resourceVersion: "20893552"
  selfLink: /apis/serving.knative.dev/v1alpha1/namespaces/default/revisions/helloworld-go-00004
  uid: bb1ed41c-a5e9-11e8-b315-00163e12479f
spec:
  concurrencyModel: Multi
  container:
    env:
    - name: TARGET
      value: Go Sample v2
    image: registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/helloworld-go
    name: ""
    resources: {}
  generation: 1
  servingState: Active
status:
  conditions:
  - lastTransitionTime: 2018-08-22T09:10:06Z
    status: "True"
    type: ResourcesAvailable
  - lastTransitionTime: 2018-08-22T09:10:06Z
    status: "True"
    type: ContainerHealthy
  - lastTransitionTime: 2018-08-22T09:10:06Z
    status: "True"
    type: Ready
  logUrl: |
    http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/discover?_a=(query:(match:(kubernetes.labels.knative-dev%2FrevisionUID:(query:'bb1ed41c-a5e9-11e8-b315-00163e12479f',type:phrase))))
  serviceName: helloworld-go-00004-service
```

![Serving](https://github.com/knative/serving/raw/master/docs/spec/images/object_model.png)