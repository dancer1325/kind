# Getting Started
## minimal Cluster config
* [here](minimalClusterConfig.yaml)
* `kind create cluster --config=minimalClusterConfig.yaml`
## create a ".yaml" / follows `Cluster struct`
* `kind create cluster --config clusterConfig.yaml`
## CLI Parameters' priority > Configuration Files' priority
* `kind create cluster --config clusterConfig.yaml --name my-cluster`
  * `kind get clusters`
    * check cluster's name == "my-cluster"
## TODO:
## Nodes
### goal: specify cluster multi node
* `kind create cluster --config multiNodeClusterConfig.yaml`
* `kind get nodes` OR `kubectl get nodes`
  * get 3 worker nodeS + 1 control plane
### by default, 1! node (== control plane)
* `kind create cluster`
* `kind get nodes` OR `kubectl get nodes`
  * get 1! node / role == "control-plane"
### TODO:
## TODO:

