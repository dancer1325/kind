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
  * how to set up an ingress | Kind cluster
    * -- through -- [cloud-provider-kind](https://github.com/kubernetes-sigs/cloud-provider-kind) v0.9.0+

* Ingress
  * allows
    * exposes HTTP & HTTPS routes | outside the cluster -- to -- services | cluster
