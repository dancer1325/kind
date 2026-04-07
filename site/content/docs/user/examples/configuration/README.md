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

# Cluster-Wide Options
## TODO:
## Networking
### IP Family
#### IPv4
* `kind create cluster --config ipv4ClusterConfiguration.yaml --name ip4cluster`
* `kubectl get svc kubernetes -o jsonpath='{.spec.clusterIP}'`
  * check it's | IPV4 range
#### IPv6
* `kind create cluster --config ipv6ClusterConfiguration.yaml --name ip6cluster`
  * Problems:
    * Problem1: "ERROR: failed to create cluster: could not find a log line that matches "Reached target .*Multi-User System.*|detected cgroup v1""
      * Attempt1: configure docker daemon / enable it
        * if you use Rancher Desktop -> | "/.rd/docker/daemon.json"
          ```json
          {                                                                                        
            "ipv6": true,                                                                          
            "fixed-cidr-v6": "2001:db8:1::/64"                                                     
          }
          ```
      * Solution: TODO:
* `kubectl get svc kubernetes -o jsonpath='{.spec.clusterIP}'`
  * check it's | IPV4 range
#### dual-stack
* `kind create cluster --config dualStackClusterConfiguration.yaml --name dualcluster`
  * Problems:
    * Problem1: "ERROR: failed to create cluster: could not find a log line that matches "Reached target .*Multi-User System.*|detected cgroup v1""
      * Attempt1: configure docker daemon / enable it
        * if you use Rancher Desktop -> | "/.rd/docker/daemon.json"
          ```json
          {                                                                                        
            "ipv6": true,                                                                          
            "fixed-cidr-v6": "2001:db8:1::/64"                                                     
          }
          ```
      * Solution: TODO:
* `kubectl get svc kubernetes -o jsonpath='{.spec.clusterIP}'`
  * TODO:
### API Server
* TODO:
### TODO:
* TODO:
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

# options / node
## Kubernetes Version
* `kind create cluster --config clusterWithKubernetesVersionConfig.yaml`
  * `docker ps | grep "kindest/node"`
    * check the node's image version

## TODO:

## Extra Port Mappings
* `kind create cluster --config clusterWithExtraPortMappings.yaml`
  * Problems:
    * Problem1: "ERROR: failed to create cluster: could not find a log line that matches "Reached target .*Multi-User System.*|detected cgroup v1""
      * Solution: `kind get clusters` & `kind delete cluster <KIND_CLUSTER_NAME>`
* `kubectl apply -f podAndNodePortService.yaml`
### allows: | your host, get traffic DIRECTLY -- to -- your kind cluster
* `curl localhost:8080`
  * 's return foo

## TODO: