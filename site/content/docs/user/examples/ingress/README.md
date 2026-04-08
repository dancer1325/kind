# requirements

* `kind create cluster`
* [install Cloud Provider KIND](https://kubernetes-sigs.github.io/cloud-provider-kind/#/?id=main)
* `sudo cloud-provider-kind`
  * == run cloud-provider-kind
  * AUTOMATICALLY enables LoadBalancer support -- for -- Ingress

# allows
## exposes HTTP & HTTPS routes | outside the cluster -- to -- services | cluster
* deploy pod + service + ingress
  * | repo host path, `kubectl apply -f site/static/examples/ingress/usage.yaml`

* `kubectl get ingress`
  * check the ADDRESS != null

* check pod's endpoints are reached

  ```
  # get the Ingress IP
  INGRESS_IP=$(kubectl get ingress example-ingress -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
  
  # | your machine, hit the pod's endpoints
  curl ${INGRESS_IP}/foo
  # 's output: foo-app
  
  curl ${INGRESS_IP}/bar
  # 's output: bar-app
  ```
