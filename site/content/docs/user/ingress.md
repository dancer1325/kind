---
title: "Ingress"
menu:
  main:
    parent: "user"
    identifier: "user-ingress"
    weight: 3
description: |-
  This guide covers setting up [ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) on a kind cluster.
---

* goal
  * [cloud-provider-kind](https://github.com/kubernetes-sigs/cloud-provider-kind) v0.9.0+
    * 💡natively supports    
      * Gateway API
      * Ingress💡
        * -> ❌NO need third-party ingress controllers❌
    * if you want to use third-party ingress solutions (_Examples:_ Ingress NGINX, Contour) -> follow their official documentation

* Ingress
  * allows
    * exposes HTTP & HTTPS routes | outside the cluster -- to -- services | cluster

# how to create the Cluster?

* `kind create cluster`
* [run Cloud Provider KIND](loadbalancer.md)
  * AUTOMATICALLY enables LoadBalancer support -- for -- Ingress 
    * Reason:🧠built-in LoadBalancer assigns External IP -- to the -- Ingress🧠
