# chartmuseum

## Packaging chart

````shell
cd demo/
helm package .
curl --data-binary "@demo-0.0.1.tgz" http://localhost:8080/api/charts
curl --data-binary "@demo-0.0.2.tgz" http://localhost:8080/api/charts
````

````shell
cd dem1/
helm package .
curl --data-binary "@demo1-0.0.1.tgz" http://localhost:8080/api/charts
curl --data-binary "@demo1-0.0.2.tgz" http://localhost:8080/api/charts
curl --data-binary "@demo1-0.0.3.tgz" http://localhost:8080/api/charts
````

## Installing Charts into Kubernetes

### Get chart from repo
```shell
helm repo list
helm repo add chartmuseum-dev http://localhost:8080
helm search repo chartmuseum-dev/demo1
helm fetch --untar --untardir ./charts chartmuseum-dev/demo1
```
### Azure example 
```shell
echo $(helm_repo_pass) | helm registry login $(helm_registry_url) --username $(helm_repo_user) --password-stdin
helm chart pull $(helmRegistryURL)/$(helmChartName):${{ parameters.chartVersion }}
helm chart export $(helmRegistryURL)/$(helmChartName):${{ parameters.chartVersion }} --destination ./install
```

### Install chart
```shell
helm upgrade --install \
  --namespace=${k8sNamespace} \
  --create-namespace \
  --wait \
  -f vars/${targetAppEnv}.yaml \
  --set image.tag=${dockerImageTag} \
  ${releaseName} chartmuseum-dev/demo1
```