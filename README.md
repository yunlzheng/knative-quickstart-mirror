Knative Quickstart Mirror
=========

Inside the wall

## Install Knative

```
kubectl create -f istio.yaml
```

```
kubectl create -f release.yaml
```

## Quick Start

[Mirror of source-to-url-go](https://github.com/knative/docs/tree/master/serving/samples/source-to-url-go)

```
apiVersion: v1
kind: Secret
metadata:
  name: basic-user-pass
  annotations:
    build.knative.dev/docker-0: https://index.docker.io/v1/
type: kubernetes.io/basic-auth
data:
  # Use 'echo -n "username" | base64' to generate this string
  username: DOCKER_HUB_USERNAME
  # Use 'echo -n "password" | base64' to generate this string 
  password: DOCKER_HUB_PASSWORD
```

```
 apiVersion: v1
 kind: ServiceAccount
 metadata:
   name: build-bot
 secrets:
 - name: basic-user-pass
```

```
apiVersion: build.knative.dev/v1alpha1
kind: BuildTemplate
metadata:
  name: kaniko
spec:
  parameters:
  - name: IMAGE
    description: The name of the image to push
  - name: DOCKERFILE
    description: Path to the Dockerfile to build.
    default: /workspace/Dockerfile

  steps:
  - name: build-and-push
    image: registry.cn-hangzhou.aliyuncs.com/kaniko-project/executor
    args:
    - --dockerfile=${DOCKERFILE}
    - --destination=${IMAGE}
```

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: app-from-source
  namespace: default
spec:
  runLatest:
    configuration:
      build:
        serviceAccountName: build-bot
        source:
          git:
            url: https://github.com/yunlzheng/simple-app.git
            revision: master
        template:
          name: kaniko
          arguments:
          - name: IMAGE
            value: &image docker.io/{USERNAME}/app-from-source
      revisionTemplate:
        spec:
          container:
            image: *image
            imagePullPolicy: Always
            env:
            - name: SIMPLE_MSG
              value: "Hello sample app!"
```