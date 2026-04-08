# requirements

* `kind create cluster`
* [install Cloud Provider KIND](https://kubernetes-sigs.github.io/cloud-provider-kind/#/?id=main)
* `sudo cloud-provider-kind`
    * == run cloud-provider-kind
    * AUTOMATICALLY enables LoadBalancer support -- for -- Ingress
## privileges -- to -- open ports | system
* TODO: why?
## connect -- to the -- container runtime
* TODO: why?

# goal
## how to set up a service / type = LoadBalancer working | Kind cluster
* | repo host path, 
  * `kubectl apply -f site/static/examples/loadbalancer/usage.yaml` 
* check the loadbalancer works
  * `kubectl get svc`
    * check service / type = Loadbalancer, has an EXTERNAL-IP
  * send traffic -- to -- it's external IP & port

    ```bash
    # output foo & bar | SEPARATE lines 
    LB_IP=$(kubectl get svc/foo-service -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')
    
    for _ in {1..10}; do
      curl ${LB_IP}:5678
    done
    ```
