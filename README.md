# Helm charts

* [Fossology](/fossology) - helm chart for a oss scanning tool

## Boot Application
### Windows

1. helm package .\fossology
2. helm upgrade -i -n fossology -f .\fossology\values.yaml fossology .\fossology-0.1.0.tgz
3. minikube service fossology-web --url -n fossology