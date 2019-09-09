## Helm Deep Dive

helm: client
tiller: server-component

helm init (initial helm on the local server)

helm init --upgrade (install tiller; need kubectl working first)

helm create NAME (create a helm chart directory structure)

helm fetch BRANCH/NAME (download the chartin tar.gz; with --untar will expand the archive)


