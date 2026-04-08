# requirements
## create the Cluster

* `kind create cluster`
* [run Cloud Provider KIND](loadbalancer.md)
  * TODO:


# allows
## exposes HTTP & HTTPS routes | outside the cluster -- to -- services | cluster
* deploy pod + service + ingress
  * | repo host path, `kubectl apply -f site/static/examples/ingress/usage.yaml`

* `kubectl get ingress`
    * check the External IP != null

* check it's reached

  ```
  # get the Ingress IP
  INGRESS_IP=$(kubectl get ingress example-ingress -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  
  # | your machine, hit the pod's endpoints
  curl ${INGRESS_IP}/foo
  curl ${INGRESS_IP}/bar
  ```
