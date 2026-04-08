---
title: "LoadBalancer"
menu:
  main:
    parent: "user"
    identifier: "user-loadbalancer"
    weight: 3
description: |-
    This guide covers how to get service of type LoadBalancer working in a kind cluster using [Cloud Provider KIND].

    This guide complements Cloud Provider KIND [installation docs].

    [Cloud Provider KIND]: https://github.com/kubernetes-sigs/cloud-provider-kind
    [installation docs]: https://github.com/kubernetes-sigs/cloud-provider-kind?tab=readme-ov-file#install

    [Ingress Guide]: /docs/user/ingress
    [Configuration Guide]: /docs/user/configuration#extra-port-mappings

---

* goal
  * how to set up a service / type = LoadBalancer working | Kind cluster
    * -- through -- [cloud-provider-kind](https://github.com/kubernetes-sigs/cloud-provider-kind) v0.9.0+

* requirements
  * privileges -- to -- open ports | system
  * connect -- to the -- container runtime 

* steps
  * [install Cloud Provider KIND](https://kubernetes-sigs.github.io/cloud-provider-kind/#/?id=main)
  * run Kubernetes service / type = LoadBalancer
