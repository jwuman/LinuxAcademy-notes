## Helm Deep Dive

helm: client
tiller: server-component

helm init (initial helm on the local server)

helm init --upgrade (install tiller; need kubectl working first)

helm repo list (list the current helm repos)

helm repo add CHART_NAME URL (the URL must contain a file index.yaml)

To build from source
github repo https://github.com/helm/helm
make sure go and glide are installed
* make bootstrap build
Add the necessary kubernetes access
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'

Create your own chart

helm create NAME (create a helm chart directory structure)

ex: helm create custom
custom
├── charts
├── Chart.yaml (meta data; version; descritpion)
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml (Where all the parameters are defined)


helm fetch BRANCH/NAME (download the chartin tar.gz; with --untar will expand the archive)


