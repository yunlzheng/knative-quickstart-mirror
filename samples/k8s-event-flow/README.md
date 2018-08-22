```
k create -f release-clusterbus-stub.yaml
clusterbus.channels.knative.dev/stub created
```

```
k apply -f release-source-k8sevents.yaml
eventsource.feeds.knative.dev/k8sevents created
eventtype.feeds.knative.dev/dev.knative.k8s.event created
```

```
k get services.serving.knative.dev
```

```
k apply -f serviceaccount.yaml
k apply -f function.yaml
k apply -f flow.yaml
```

## Kibana

http://localhost:8001/api/v1/namespaces/monitoring/services/kibana-logging/proxy/app/kibana#/management/kibana/index?_g=()
