Pod Example
=======

```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    sidecar.istio.io/inject: "false"
  labels:
    build-name: test-git-branch
  name: test-git-branch-9tplb
  namespace: default
spec:
  containers:
  - args:
    - Nothing to push
    command:
    - /bin/echo
    image: busybox
    imagePullPolicy: Always
    name: nop
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-tzpfg
      readOnly: true
  dnsPolicy: ClusterFirst
  initContainers:
  - env:
    - name: HOME
      value: /builder/home
    image: registry.cn-hangzhou.aliyuncs.com/build-crd/creds-init:v0.1.0
    name: build-step-credential-initializer
    volumeMounts:
    - mountPath: /workspace
      name: workspace
    - mountPath: /builder/home
      name: home
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-tzpfg
      readOnly: true
    workingDir: /workspace
  - args:
    - -url
    - https://github.com/bazelbuild/rules_docker
    - -revision
    - master
    env:
    - name: HOME
      value: /builder/home
    image: registry.cn-hangzhou.aliyuncs.com/build-crd/git-init:v0.1.0
    imagePullPolicy: IfNotPresent
    name: build-step-git-source
    volumeMounts:
    - mountPath: /workspace
      name: workspace
    - mountPath: /builder/home
      name: home
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-tzpfg
      readOnly: true
    workingDir: /workspace
  - args:
    - cat
    - WORKSPACE
    env:
    - name: HOME
      value: /builder/home
    image: ubuntu:latest
    name: build-step-dump-workspace
    volumeMounts:
    - mountPath: /workspace
      name: workspace
    - mountPath: /builder/home
      name: home
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-tzpfg
      readOnly: true
    workingDir: /workspace
  restartPolicy: Never
  volumes:
  - emptyDir: {}
    name: workspace
  - emptyDir: {}
    name: home
  - name: default-token-tzpfg
    secret:
      defaultMode: 420
      secretName: default-token-tzpfg
```