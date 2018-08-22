## Image List

### Build

|  mirror   |   source  |
|--------|-----------|
| registry.cn-hangzhou.aliyuncs.com/build-crd/creds-init:v0.1.0 | gcr.io/build-crd/github.com/knative/build/cmd/creds-init@sha256:cfcc14889abe29b54d17ff6cf414918d9d92ff02bb112525742ec2e30117899f  |
| registry.cn-hangzhou.aliyuncs.com/build-crd/git-init:v0.1.0   | gcr.io/build-crd/github.com/knative/build/cmd/git-init@sha256:d52c29a4a1d83712b048bd32dbb5f97b7d2791af9b892f68fe94e162932ac66e  |
| registry.cn-hangzhou.aliyuncs.com/build-crd/controller:v0.1.0 | gcr.io/build-crd/github.com/knative/build/cmd/controller@sha256:9220968c9aae7e6edac97effc7b693fcf5bbb17edf78aa14347de87b15ac8840  |
| registry.cn-hangzhou.aliyuncs.com/build-crd/webhook:v0.1.0    | gcr.io/build-crd/github.com/knative/build/cmd/webhook@sha256:a7ed8fb8828f71a6aba3f9f9899eff6c867ceb5a8ceeaed903008c2296f919fb  |

### Serving

|  mirror   |   source  |
|--------|-----------|
| registry.cn-hangzhou.aliyuncs.com/knative-releases/activator:v0.1.0 | gcr.io/knative-releases/github.com/knative/serving/cmd/activator@sha256:dfe87fbf1cc25f45648f78b04b25a42b7c5a54267f23911b8576fd326d250237|
| registry.cn-hangzhou.aliyuncs.com/knative-releases/controller:v0.1.0| gcr.io/knative-releases/github.com/knative/serving/cmd/controller@sha256:d965a7ce7eea3463ede13b715019562ab0a32bfe0e4965c2f0e2f48221d48609|
| registry.cn-hangzhou.aliyuncs.com/knative-releases/webhook:v0.1.0| gcr.io/knative-releases/github.com/knative/serving/cmd/webhook@sha256:43b111b4bb74853cb2aabbfc6eda8a6998f434e8192d3054479e0ec44d13a847 |
| registry.cn-hangzhou.aliyuncs.com/knative-releases/autoscaler:v0.1.0| gcr.io/knative-releases/github.com/knative/serving/cmd/autoscaler@sha256:fd92d915b4e275c0c28e05ad812d6d5c0159339a1b380a7acb79d9cd0244de15 |
| registry.cn-hangzhou.aliyuncs.com/knative-releases/queue:v0.1.0 | gcr.io/knative-releases/github.com/knative/serving/cmd/queue@sha256:90a46fd308cd21ee292bd3159bf84c75d0bb1dc4539fd61b814a3ebd8b74db4b |

### Eventing

| mirror | source |
|--------|--------|
|   registry.cn-hangzhou.aliyuncs.com/knative-releases/eventing-buses-stub:v0.1.0     |   gcr.io/knative-releases/github.com/knative/eventing/pkg/buses/stub@sha256:69097e75810496684dfb01949381b5f3d31e0b6a8f7da7a28217456a53c4fa32 |
|   registry.cn-hangzhou.aliyuncs.com/knative-releases/eventing-k8sevents:v1.0.1 | gcr.io/knative-releases/github.com/knative/eventing/pkg/sources/k8sevents@sha256:2e5f9aa9c0825a7e72436a3169ffb4becab16600e1319487b4ed76a992364649 |
|   registry.cn-hangzhou.aliyuncs.com/knative-releases/eventing-receive_adapter:v0.1.0 | gcr.io/knative-releases/github.com/knative/eventing/pkg/sources/k8sevents/receive_adapter@sha256:a310f738488222f0abcec1880ad2d203d68270680e49b1b8228f2242d86c225d|
|  registry.cn-hangzhou.aliyuncs.com/knative-releases/eventing-controller:v0.1.0 |  gcr.io/knative-releases/github.com/knative/eventing/cmd/controller@sha256:36b930ac02e6737e3542646b09e7967ba3e28182c88ade793915663940861289 |
|  registry.cn-hangzhou.aliyuncs.com/knative-releases/eventing-webhook:v0.1.0 |  gcr.io/knative-releases/github.com/knative/eventing/cmd/webhook@sha256:4c2ff8751ac59d0c185f79d2984d264c73611f9ff66836857ee1a61ec14e53ec|

### Istio

| mirror | source |
|--------|--------|
| registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/hyperkube:v1.7.6_coreos.0 |   quay.io/coreos/hyperkube:v1.7.6_coreos.0 |
| registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/pilot:0.8.0 | docker.io/istio/pilot:0.8.0 |


### Othersls

|  mirror   |   source  |
|--------|-----------|
|registry.cn-hangzhou.aliyuncs.com/kaniko-project/executor| gcr.io/kaniko-project/executor |
|registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/fluentd-elasticsearch:v2.0.4| k8s.gcr.io/fluentd-elasticsearch:v2.0.4 |
|registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/addon-resizer:1.7| k8s.gcr.io/addon-resizer:1.7 |
|registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/elasticsearch:v5.6.4| k8s.gcr.io/elasticsearch:v5.6.4 |
|registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/helloworld-go| gcr.io/knative-samples/helloworld-go |
|registry.cn-hangzhou.aliyuncs.com/k8s-mirrors/cloud-builders-docker | gcr.io/cloud-builders/docker |