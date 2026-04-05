---
title: "Configuration"
menu:
  main:
    parent: "user"
    identifier: "user-configuration"
    weight: 3
toc: true
description: |-
  This guide covers how to configure KIND cluster creation.
  
  We know this is currently a bit lacking and will expand it over time - PRs welcome!
---
## Getting Started

* goal
  * how to configure kind cluster creation?

* steps
  * create a ".yaml" /
    * 👀follows
      * Kubernetes versioning conventions 👀
        * DIFFERENT options & behaviors / version
      * [`Cluster struct`](/pkg/apis/config/v1alpha4/types.go)
    * _Example:_ [minimal one](examples/configuration/minimalClusterConfig.yaml)
  * `kind create cluster --config=<RELATIVE_PATH_YOUR_FILE_NAME>.yaml`

* CLI Parameters' priority > Configuration Files' priority

## Cluster-Wide Options

* | "*.yaml"'s `kind: Cluster`,
  * `.name`
  * `.featureGates`
    * == [Kubernetes feature gates](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/) / 
      * to enable 
      * apply | ALL Kubernetes' components

### Runtime Config
TODO: 
Kubernetes API server runtime-config can be toggled using the `runtimeConfig`
key, which maps to the `--runtime-config` [kube-apiserver flag](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/).
This may be used to e.g. disable beta / alpha APIs, or even enable deprecated APIs.


### Networking

#### IP Family

* supported ones
  * IPv4
    * default one
  * IPv6
  * dual-stack

##### IPv6 clusters

* ⚠️requirements⚠️
  * host / runs the docker containers
    * support IPv6
      * | MOST OS OR distros, IPv6 is enabled by default
      * way to check
        * | Linux

          ```sh
          sudo sysctl net.ipv6.conf.all.disable_ipv6
          # COMMON output
          #   net.ipv6.conf.all.disable_ipv6 = 0          
          ```
  * | Docker | Windows or Mac,
    * use an IPv4 port forward -- for the -- API Server
      * Reason:🧠IPv6 port forwards do NOT work | these platforms🧠 you can do this with the following config:

##### Dual Stack clusters
* requirements
  * kind v0.11+
  * Kubernetes v1.20+

#### API Server

The API Server listen address and port can be customized with


```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  # WARNING: It is _strongly_ recommended that you keep this the default
  # (127.0.0.1) for security reasons. However it is possible to change this.
  apiServerAddress: "127.0.0.1"
  # By default the API server listens on a random open port.
  # You may choose a specific port but probably don't need to in most cases.
  # Using a random port makes it easier to spin up multiple clusters.
  apiServerPort: 6443
```

{{< securitygoose >}}**NOTE**: You should really think thrice before exposing your kind cluster publicly!
kind does not ship with state of the art security or any update strategy (other than
disposing your cluster and creating a new one)! We strongly discourage exposing kind
to anything other than loopback.{{</ securitygoose >}}

#### Pod Subnet

You can configure the subnet used for pod IPs by setting

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  podSubnet: "10.244.0.0/16"
```

By default, kind uses ```10.244.0.0/16``` pod subnet for IPv4 and ```fd00:10:244::/56``` pod subnet for IPv6.

#### Service Subnet

You can configure the Kubernetes service subnet used for service IPs by setting

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  serviceSubnet: "10.96.0.0/12"
```

By default, kind uses ```10.96.0.0/16``` service subnet for IPv4 and ```fd00:10:96::/112``` service subnet for IPv6.

#### Disable Default CNI

KIND ships with a simple networking implementation ("kindnetd") based around
standard CNI plugins (`ptp`, `host-local`, ...) and simple netlink routes.

This CNI also handles IP masquerade.

You may disable the default to install a different CNI
* This is a power user
feature with limited support, but many common CNI manifests are known to work,
e.g. Calico.
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  # the default CNI will not be installed
  disableDefaultCNI: true
```


#### kube-proxy mode

You can configure the kube-proxy mode that will be used, between iptables, nftables (Kubernetes v1.31+), and ipvs.
By default iptables is used

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  kubeProxyMode: "nftables"
```

To disable kube-proxy, set the mode to `"none"`.

### Nodes

* goal
  * specify cluster multi node

* | "*.yaml" / `kind: Cluster`
  * specify `nodes`
* by default, 
  * 1! node (== control plane)

* use cases
  * testing Kubernetes

## options / node

### Kubernetes Version

You can set a specific Kubernetes version by setting the `node`'s container image
* You can find available image tags on the [releases page](https://github.com/kubernetes-sigs/kind/releases)
* Please include the `@sha256:` [image digest](https://docs.docker.com/engine/reference/commandline/pull/#pull-an-image-by-digest-immutable-identifier) from the image in the release notes, as seen in this example:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: kindest/node:v1.16.4@sha256:b91a2c2317a000f3a783489dfb755064177dbc3a0b2f4147d50f04825d016f55
- role: worker
  image: kindest/node:v1.16.4@sha256:b91a2c2317a000f3a783489dfb755064177dbc3a0b2f4147d50f04825d016f55
```

[Reference](https://kind.sigs.k8s.io/docs/user/quick-start/#creating-a-cluster) 

**Note**: Kubernetes versions are expressed as x.y.z, where x is the major version, y is the minor version, and z is the patch version, following [Semantic Versioning](https://semver.org/) terminology
* For more information, see [Kubernetes Release Versioning.](https://github.com/kubernetes/sig-release/blob/master/release-engineering/versioning.md#kubernetes-release-versioning)

### Extra Mounts

Extra mounts can be used to pass through storage on the host to a kind node
for persisting data, mounting through code etc.

{{< codeFromFile file="static/examples/config-with-mounts.yaml" lang="yaml" >}}


**NOTE**: If you are using Docker for Mac or Windows check that the hostPath is
included in the Preferences -> Resources -> File Sharing.

For more information see the [Docker file sharing guide.](https://docs.docker.com/docker-for-mac/#file-sharing)

### Extra Port Mappings

Extra port mappings can be used to port forward to the kind nodes
* This is a 
cross-platform option to get traffic into your kind cluster. 

If you are running Docker without the Docker Desktop Application on Linux, you can simply send traffic to the node IPs from the host without extra port mappings. 
With the installation of the Docker Desktop Application, whether it is on macOs, Windows or Linux, you'll want to use these.

You may also want to see the [Ingress Guide].

> **NOTE**: If you're running Kind on a remote host and need to send traffic to Kind node
> IPs from a different host than where kind is running, you need to configure port-mapping.

{{< codeFromFile file="static/examples/config-with-port-mapping.yaml" lang="yaml" >}}

An example http pod mapping host ports to a container port.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: foo
spec:
  containers:
  - name: foo
    image: hashicorp/http-echo:0.2.3
    args:
    - "-text=foo"
    ports:
    - containerPort: 5678
      hostPort: 80
```

#### NodePort with Port Mappings

To use port mappings with `NodePort`, the kind node `containerPort` and the service `nodePort` needs to be equal.

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30950
    hostPort: 80
```

And then set `nodePort` to be 30950.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: foo
  labels:
    app: foo
spec:
  containers:
  - name: foo
    image: hashicorp/http-echo:0.2.3
    args:
    - "-text=foo"
    ports:
    - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: foo
spec:
  type: NodePort
  ports:
  - name: http
    nodePort: 30950
    port: 5678
  selector:
    app: foo
```

[Ingress Guide]: /docs/user/ingress

### Extra Labels

Extra labels might be useful for working with
[nodeSelectors](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/).

An example label for specifying a `tier` label:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
  - containerPort: 30950
    hostPort: 80
  labels:
    tier: frontend
- role: worker
  labels:
    tier: backend
```

### Kubeadm Config Patches

KIND uses [`kubeadm`](/docs/design/principles/#leverage-existing-tooling) 
to configure cluster nodes.

Formally  KIND runs `kubeadm init` on the first control-plane node, we can customize the flags by using the kubeadm
[InitConfiguration](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/#config-file) 
([spec](https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta3#InitConfiguration))

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "my-label=true"
```

If you want to do more customization, there are four configuration types available during `kubeadm init`: `InitConfiguration`, `ClusterConfiguration`, `KubeProxyConfiguration`, `KubeletConfiguration`
* For example, we could override the apiserver flags by using the kubeadm [ClusterConfiguration](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/control-plane-flags/) ([spec](https://pkg.go.dev/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta3#ClusterConfiguration)):

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: ClusterConfiguration
    apiServer:
        extraArgs:
          enable-admission-plugins: NodeRestriction,MutatingAdmissionWebhook,ValidatingAdmissionWebhook
```

> **NOTE**: When using `KubeletConfiguration`, kubeadm only reads the kubelet configuration from the **first** node, which will apply to all nodes.
> This is a current [limitation](https://github.com/kubernetes-sigs/kind/issues/3849).

As a result, if you want to change the kubelet's configuration for any additional node, such as applying a taint, you must use `JoinConfiguration`:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: test
nodes:
- role: control-plane
- role: worker
  kubeadmConfigPatches:
    - |
      kind: JoinConfiguration
      nodeRegistration:
        kubeletExtraArgs:
          register-with-taints: "my-taint=presence:NoSchedule"
```

On every additional node configured in the KIND cluster, 
worker or control-plane (in HA mode),
KIND runs `kubeadm join` which can be configured using the 
[JoinConfiguration](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/#config-file)
([spec](https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta3#JoinConfiguration))

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
  kubeadmConfigPatches:
  - |
    kind: JoinConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "my-label2=true"
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: JoinConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "my-label3=true"
```

If you need more control over patching, strategic merge and JSON6092 patches can
be used as well
* These are specified using files in a directory, for example
`./patches/kube-controller-manager.yaml` could be the following.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kube-controller-manager
  namespace: kube-system
spec:
  containers:
  - name: kube-controller-manager
    env:
    - name: KUBE_CACHE_MUTATION_DETECTOR
      value: "true"
```

Then in your kind YAML configuration use the following.

```yaml
nodes:
- role: control-plane
  extraMounts:
  - hostPath: ./patches
    containerPath: /patches

kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    patches:
      directory: /patches
```

Note the `extraMounts` stanza
* The node is a container created by
`kind`
* `kubeadm` is run inside this node container, and the local directory
that contains the patches has to be accessible to `kubeadm`
* `extraMounts`
plumbs a local directory through to this node container.

This example was for changing the manager in the control plane
* To use a patch
for a worker node, use a `JoinConfiguration` patch and an `extraMounts` stanza
for the `worker` role.

[YAML]: https://yaml.org/
