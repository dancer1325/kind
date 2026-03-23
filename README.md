<p align="center"><img alt="kind" src="./logo/logo.png" width="300px" /></p>


* kind
  * 👀 == tool for running -- via Docker container "nodes" -- local Kubernetes clusters 👀
    * each "node" -- is bootstrapped with -- [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/)
  * origin
    * testing Kubernetes itself
  * uses 
    * local development
    * CI
  * == 
    * Go [packages](./pkg) / implement 
      * [cluster creation](./pkg/cluster)
      * [image build](./pkg/build)
      * etc.
    * CL interface ([`kind`](./main.go)) /
      * built | previous packages 
    * Docker [image(s)](./images) / 
      * run 
        * systemd
        * Kubernetes
        * etc
    * [`kubetest`](https://github.com/kubernetes/test-infra/tree/master/kubetest) integration / built | previous packages (WIP)
  * WIP
    * [1.0 roadmap](/site/content/docs/contributing/1.0-roadmap.md)
  * [documentation](site)
  * supports
    * multi-node (including HA) clusters
    * ANY OS (Linux, macOS and Windows)
  - ❌NOT require an additional VM❌
    - Reason: 🧠 runs as a linux container /
      - has a set of processes
      - Linux kernel used == Linux kernel -- used by -- Docker daemon🧠
    - 👀!= local installation of Minikube or Minishift
      - ❌Kubernetes cluster is started | VM❌
        - == run a Linux kernel + Docker daemon -> consume EXTRA CPU and RAM
          - -> kind is faster & consumes less CPU and RAM

## Community

* CURRENT maintainers
  * are
    * [@aojea]
    * [@BenTheElder]
    * [@stmcginnis]
  * reachable -- via --
    * [Kubernetes Slack](http://slack.k8s.io/) | #kind channel
    * [filing an issue](https://github.com/kubernetes-sigs/kind/issues/new)
    * Kubernetes [SIG-Testing Mailing List](https://groups.google.com/forum/#!forum/kubernetes-sig-testing)
