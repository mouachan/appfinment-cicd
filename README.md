# appfinment-cicd
Jenkins Pipeline

Create a git secret 

```sh
oc create secret generic github --from-literal=username=user --from-literal=password=password
```
Synchronise Openshift secret with Jenkins plugins

```sh
oc label secret github credential.sync.jenkins.openshift.io=true
```

Create a build config on openshift with git source and 
```yaml
apiVersion: build.openshift.io/v1
kind: BuildConfig
metadata:
  creationTimestamp: "2020-03-30T09:45:28Z"
  name: appfinment-bc
  namespace: appfinment-cicd
  resourceVersion: "3725789"
  selfLink: /apis/build.openshift.io/v1/namespaces/appfinment-cicd/buildconfigs/appfinment-bc
  uid: 9eb798d8-4051-4580-be8e-59b253170239
spec:
  failedBuildsHistoryLimit: 5
  nodeSelector: {}
  output: {}
  postCommit: {}
  resources: {}
  runPolicy: Serial
  source:
    git:
      ref: master
      uri: https://github.com/mouachan/appfinment-cicd.git
    sourceSecret:
      name: github
    type: Git
  strategy:
    jenkinsPipelineStrategy:
      jenkinsfilePath: jenkinsFile
    type: JenkinsPipeline
  successfulBuildsHistoryLimit: 5
  triggers: []
status:
  lastVersion: 43
```

