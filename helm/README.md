# Helm Deep Dive

## Install helm/tiller

helm: client
tiller: server-component

### Install binary

Install from the instructions on the github page
<https://github.com/helm/helm/blob/master/docs/install.md>

### Initialize the environment

helm init (initial helm on the local server)

helm init --upgrade (install tiller; need kubectl working first)

helm repo list (list the current helm repos)

helm repo add CHART_NAME URL (the URL must contain a file index.yaml)

### To build from source

github repo <https://github.com/helm/helm>

make sure go and glide are installed

`make bootstrap build`

Add the necessary kubernetes access

`kubectl create serviceaccount --namespace kube-system tiller`

`kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller`

`kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'`

## Create your own chart

helm create NAME (create a helm chart directory structure)

ex: helm create custom

```console
custom
├── charts
├── Chart.yaml (meta data; version; descritpion)
├── templates
│   ├── deployment.yaml
│   ├── \_helpers.tpl
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── service.yaml
│   └── tests
│    └── test-connection.yaml
└── values.yaml (Where all the parameters are defined)
```

helm fetch BRANCH/NAME (download the chartin tar.gz; with --untar will expand the archive)

helm index

helm package

helm repo

### Install Charts to Kubernetes

helm install LOCAL_DIR or from a repo (ex: helm install ./mychart; helm install stable/wordpress)

### LAB: Using Helm Charts

helm search wordpress

helm fetch --untar stable/wordpress

cd wordpress

change persistence from enabled: true to enabled: false in values.yaml (two places: wordpress/mariadb)
change service type to NodePort and http port to 30080

helm install wordpress

helm ls --short

helm delete <RELEASE_NAME>

helm ls

kubectl get pv

kubectl get pvc

### LAB: Creating a Release in Helm

cd nginx
change the index.html to a different msg
change service type to NodePort and change the nodePort to 30080

change the version to 0.2.0 in Charts.yaml

helm install nginx

helm ls --short

helm history <RELEASE_NAME>

change the index.html to something else
change version to 0.3.0 in Charts.yaml

helm ls --short

helm upgrade <RELEASE_NAME> nginx

helm history <RELEASE_NAME>

helm ls --short

helm rollback <RELEASE_NAME> 1

helm history <RELEASE_NAME>
