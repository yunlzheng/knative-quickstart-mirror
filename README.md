Knative Quickstart Mirror
=========

Inside the wall

## Install Knative

> Docker Image Mirror see [Mirrors.md](./Mirrors.md)

```
kubectl create -f istio.yaml
```

```
kubectl create -f release.yaml
```

## Quick Start

[Mirror of source-to-url-go](https://github.com/knative/docs/tree/master/serving/samples/source-to-url-go)

### Deploy Application

1, Create a new Secret manifest, which is used to store your Docker Hub credentials. Save this file as docker-secret.yaml:

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

2. Create a new Service Account manifest which is used to link the build process to the secret. Save this file as service-account.yaml

```
 apiVersion: v1
 kind: ServiceAccount
 metadata:
   name: build-bot
 secrets:
 - name: basic-user-pass
```

3. After you have created the manifest files, apply them to your cluster with kubectl

```
$ kubectl apply -f docker-secret.yaml
secret "basic-user-pass" created
$ kubectl apply -f service-account.yaml
serviceaccount "build-bot" created
```

4. Install the kaniko build template

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

5. You need to create a service manifest which defines the service to deploy, including where the source code is and which build-template to use. Create a file named service.yaml and copy the following definition. Make sure to replace {DOCKER_USERNAME} with your own Docker Hub username

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

6. Apply this manifest using kubectl, and watch the results:

```
$ kubectl apply -f service.yaml
service "app-from-source" created
```

### Visit Application

1. To check on the state of the service, get the service object and examine the status block

```
$ kubectl get service.serving.knative.dev app-from-source -o yaml

  conditions:
  - lastTransitionTime: 2018-08-09T03:29:49Z
    status: "True"
    type: RoutesReady
  - lastTransitionTime: 2018-08-09T03:34:36Z
    message: 'Revision "app-from-source-00001" failed with message: "".'
    reason: RevisionFailed
    status: "False"
    type: ConfigurationsReady
  - lastTransitionTime: 2018-08-09T03:34:36Z
    message: 'Revision "app-from-source-00001" failed with message: "".'
    reason: RevisionFailed
    status: "False"
    type: Ready
  domain: app-from-source.default.example.com
  domainInternal: app-from-source.default.svc.cluster.local
  latestCreatedRevisionName: app-from-source-00001

```

2. To get the ingress IP for your cluster, use the following command. If your cluster is new, it can take some time for the service to get an external IP address:

```
$ kubectl get svc knative-ingressgateway -n istio-system
NAME                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)                                      AGE
knative-ingressgateway   LoadBalancer   172.19.9.80   47.95.183.249   80:32380/TCP,443:32390/TCP,32400:32400/TCP   16h
```

3. To find the URL for your service, type:

```
$ kubectl get services.serving.knative.dev app-from-source  -o=custom-columns=NAME:.metadata.name,DOMAIN:.status.domain
NAME              DOMAIN
app-from-source   app-from-source.default.example.com
```

4. Now you can make a request to your app to see the result. Replace {IP_ADDRESS} with the address that you got in the previous step:

```
$ curl -H "Host: app-from-source.default.example.com" http://47.95.183.249
```