# `type Cluster struct {`
* == kind cluster configuration 
* ``Name string `yaml:"name,omitempty" json:"name,omitempty"``
  * == cluster name
  * OPTIONAL
    * by default, kind
  * 's priority < `KIND_CLUSTER_NAME` environment variable 's priority < `--name` CL flag's priority
* ``Nodes []Node `yaml:"nodes,omitempty" json:"nodes,omitempty"``
  * == cluster's nodes
  * OPTIONAL
    * by default, 1! (== control plane)
    * if you specify >1 control plane -> provision IMPLICITLY an EXTERNAL control plane load balancer 
* ``Networking Networking `yaml:"networking,omitempty" json:"networking,omitempty"``
  * [here](#type-networking-struct-)
* ``FeatureGates map[string]bool `yaml:"featureGates,omitempty" json:"featureGates,omitempty"``
  * == [Kubernetes feature gates](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/) / 
    * to enable
    * apply | ALL Kubernetes' components
* ``RuntimeConfig map[string]string `yaml:"runtimeConfig,omitempty" json:"runtimeConfig,omitempty"``
  * TODO: // RuntimeConfig Keys and values are translated into --runtime-config values for kube-apiserver, separated by commas.
      //
      // Use this to enable alpha APIs.

* ``KubeadmConfigPatches []string `yaml:"kubeadmConfigPatches,omitempty" json:"kubeadmConfigPatches,omitempty"``
  * == inline yaml blob-string
  * https://tools.ietf.org/html/rfc7386
  * apply | generated kubeadm config
    * == merge patches
  * applying order
    * cluster-level patches
    * [node-level patches](#type-node-struct-)
  * TODO: `kind` field must match the target object, and if `apiVersion` is specified it will only be applied to matching objects.

* ``KubeadmConfigPatchesJSON6902 []PatchJSON6902 `yaml:"kubeadmConfigPatchesJSON6902,omitempty" json:"kubeadmConfigPatchesJSON6902,omitempty"``
  *         // KubeadmConfigPatchesJSON6902 are applied to the generated kubeadm config
        // as JSON 6902 patches. The `kind` field must match the target object, and
        // if group or version are specified it will only be objects matching the
        // apiVersion: group+"/"+version
        //
        // Name and Namespace are now ignored, but the fields continue to exist for
        // backwards compatibility of parsing the config. The name of the generated
        // config was/is always fixed as is the namespace so these fields have
        // always been a no-op.
        //
        // https://tools.ietf.org/html/rfc6902
        //
        // The cluster-level patches are applied before the node-level patches.

* ``ContainerdConfigPatches []string `yaml:"containerdConfigPatches,omitempty" json:"containerdConfigPatches,omitempty"``
  *           // ContainerdConfigPatches are applied to every node's containerd config
          // in the order listed.
          // These should be toml stringsto be applied as merge patches
          // If the version field in these patches doesn't match containerd config, it will not be a applied
          // This way you can write configurations that work for both by supplying two patches

* ``ContainerdConfigPatchesJSON6902 []string `yaml:"containerdConfigPatchesJSON6902,omitempty" json:"containerdConfigPatchesJSON6902,omitempty"``
  *             // ContainerdConfigPatchesJSON6902 are applied to every node's containerd config
            // in the order listed.
            // These should be YAML or JSON formatting RFC 6902 JSON patches
            // NOTE: These are not currently version-aware.

# `type TypeMeta struct {`
* == ⚠️PARTIALLY⚠️ "apimachinery/pkg/apis/meta/v1"'s `TypeMeta` 
  * == ONLY 
    * ``Kind       string `yaml:"kind,omitempty" json:"kind,omitempty"``
    * ``APIVersion string `yaml:"apiVersion,omitempty" json:"apiVersion,omitempty"``
  * Reason to avoid using the DIRECT dependency: 🧠these fields are PRETTY stable == IMPROBABLE to be removed🧠

# `type Node struct {`
* == node settings
* == Docker's container / install ALL components -- based on the -- role
  * _Examples:_
    * if node's role == control plane -> install control plane's required components
    * if node's role == worker -> install worker's required components
* ``Role NodeRole `yaml:"role,omitempty" json:"role,omitempty"``
  * by default, "control-plane"
* ``Image string `yaml:"image,omitempty" json:"image,omitempty"``
  * == node image / uses | create this node
  * OPTIONAL
    * by default, [this image](../defaults/image.go)
* ``Labels map[string]string `yaml:"labels,omitempty" json:"labels,omitempty"``
  * == node's labels
* ``ExtraMounts []Mount `yaml:"extraMounts,omitempty" json:"extraMounts,omitempty"``
  * == node container's ADDITIONAL mount points 
  * if you specify cri-like types -> should be inline
  * uses
    * bind a hostPath
* ``ExtraPortMappings []PortMapping `yaml:"extraPortMappings,omitempty" json:"extraPortMappings,omitempty"``
  * == node container's ADDITIONAL port mappings /
    * binded -- to a -- host Port
* ``KubeadmConfigPatches []string `yaml:"kubeadmConfigPatches,omitempty" json:"kubeadmConfigPatches,omitempty"``
  * == inline yaml blob-string
  * https://tools.ietf.org/html/rfc7386
  * apply | generated kubeadm config
    * == merge patches
  * TODO: The `kind` field must match the target object, and if `apiVersion` is specified it will only be applied to matching objects.
  * applying order
    * [cluster-level patches](#type-cluster-struct-)
    * node-level patches
  * 
              

                  // KubeadmConfigPatchesJSON6902 are applied to the generated kubeadm config
                  // as JSON 6902 patches. The `kind` field must match the target object, and
                  // if group or version are specified it will only be objects matching the
                  // apiVersion: group+"/"+version
                  //
                  // Name and Namespace are now ignored, but the fields continue to exist for
                  // backwards compatibility of parsing the config. The name of the generated
                  // config was/is always fixed as is the namespace so these fields have
                  // always been a no-op.
                  //
                  // https://tools.ietf.org/html/rfc6902
                  //
                  // The node-level patches will be applied after the cluster-level patches
                  // have been applied. (See Cluster.KubeadmConfigPatchesJSON6902)
                  KubeadmConfigPatchesJSON6902 []PatchJSON6902 `yaml:"kubeadmConfigPatchesJSON6902,omitempty" json:"kubeadmConfigPatchesJSON6902,omitempty"`


// NodeRole defines possible role for nodes in a Kubernetes cluster managed by `kind`
# `type NodeRole string`

const (
	// ControlPlaneRole identifies a node that hosts a Kubernetes control-plane.
	// NOTE: in single node clusters, control-plane nodes act also as a worker
	// nodes, in which case the taint will be removed. see:
	// https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#control-plane-node-isolation
	ControlPlaneRole NodeRole = "control-plane"
	// WorkerRole identifies a node that hosts a Kubernetes worker
	WorkerRole NodeRole = "worker"
)

 
# `type Networking struct {`
* == network settings
  * cluster-wide

* ``IPFamily ClusterIPFamily `yaml:"ipFamily,omitempty" json:"ipFamily,omitempty"``
  * == network cluster model
    
* ``APIServerPort int32 `yaml:"apiServerPort,omitempty" json:"apiServerPort,omitempty"``
  * == host's listen port -- for the -- Kubernetes API Server
    * by default,
      * a random port
    * if you set `-1` -> node backend (docker, podman...) pick the port
  * use cases
    * remote hosts
  * TODO: BUT it means when the container is restarted it will be randomized
      
  
* ``APIServerAddress string `yaml:"apiServerAddress,omitempty" json:"apiServerAddress,omitempty"``
  * == host's listen address -- for the -- Kubernetes API Server
  * == IP address
    * by default,
      * 127.0.0.1
      
// PodSubnet is the CIDR used for pod IPs
      // kind will select a default if unspecified
      PodSubnet string `yaml:"podSubnet,omitempty" json:"podSubnet,omitempty"`

      // ServiceSubnet is the CIDR used for services VIPs
      // kind will select a default if unspecified for IPv6
      ServiceSubnet string `yaml:"serviceSubnet,omitempty" json:"serviceSubnet,omitempty"`

      // If DisableDefaultCNI is true, kind will not install the default CNI setup.
      // Instead the user should install their own CNI after creating the cluster.
      DisableDefaultCNI bool `yaml:"disableDefaultCNI,omitempty" json:"disableDefaultCNI,omitempty"`

      // KubeProxyMode defines if kube-proxy should operate in iptables, ipvs or nftables mode
      // Defaults to 'iptables' mode
      KubeProxyMode ProxyMode `yaml:"kubeProxyMode,omitempty" json:"kubeProxyMode,omitempty"`

      // DNSSearch defines the DNS search domain to use for nodes. If not set, this will be inherited from the host.
      DNSSearch *[]string `yaml:"dnsSearch,omitempty" json:"dnsSearch,omitempty"`
  }


# `type ClusterIPFamily string`
* == cluster network IP family 
```
const (
	// ClusterIPFamily == ipv4
	IPv4Family ClusterIPFamily = "ipv4"
	
	// ClusterIPFamily == ipv6
	IPv6Family ClusterIPFamily = "ipv6"
	
	// ClusterIPFamily == dual
	DualStackFamily ClusterIPFamily = "dual"
)
```


# `type ProxyMode string`
* == proxy mode
  * uses
    * by kube-proxy 

const (
	// IPTablesProxyMode sets ProxyMode to iptables
	IPTablesProxyMode ProxyMode = "iptables"
	// IPVSProxyMode sets ProxyMode to ipvs
	IPVSProxyMode ProxyMode = "ipvs"
	// NFTablesProxyMode sets ProxyMode to nftables
	NFTablesProxyMode ProxyMode = "nftables"
)

// PatchJSON6902 represents an inline kustomize json 6902 patch
// https://tools.ietf.org/html/rfc6902
# `type PatchJSON6902 struct {`
	// these fields specify the patch target resource
	Group   string `yaml:"group" json:"group"`
	Version string `yaml:"version" json:"version"`
	Kind    string `yaml:"kind" json:"kind"`
	// Patch should contain the contents of the json patch as a string
	Patch string `yaml:"patch" json:"patch"`
}

/*
These types are from
https://github.com/kubernetes/kubernetes/blob/063e7ff358fdc8b0916e6f39beedc0d025734cb1/pkg/kubelet/apis/cri/runtime/v1alpha2/api.pb.go#L183
*/

// Mount specifies a host volume to mount into a container.
// This is a close copy of the upstream cri Mount type
// see: k8s.io/kubernetes/pkg/kubelet/apis/cri/runtime/v1alpha2
// It additionally serializes the "propagation" field with the string enum
// names on disk as opposed to the int32 values, and the serialized field names
// have been made closer to core/v1 VolumeMount field names
// In yaml this looks like:
//
//	containerPath: /foo
//	hostPath: /bar
//	readOnly: true
//	selinuxRelabel: false
//	propagation: None
//
// Propagation may be one of: None, HostToContainer, Bidirectional
# `type Mount struct {`
	// Path of the mount within the container.
	ContainerPath string `yaml:"containerPath,omitempty" json:"containerPath,omitempty"`
	// Path of the mount on the host. If the hostPath doesn't exist, then runtimes
	// should report error. If the hostpath is a symbolic link, runtimes should
	// follow the symlink and mount the real destination to container.
	HostPath string `yaml:"hostPath,omitempty" json:"hostPath,omitempty"`
	// If set, the mount is read-only.
	Readonly bool `yaml:"readOnly,omitempty" json:"readOnly,omitempty"`
	// If set, the mount needs SELinux relabeling.
	SelinuxRelabel bool `yaml:"selinuxRelabel,omitempty" json:"selinuxRelabel,omitempty"`
	// Requested propagation mode.
	Propagation MountPropagation `yaml:"propagation,omitempty" json:"propagation,omitempty"`
}

// PortMapping specifies a host port mapped into a container port.
// In yaml this looks like:
//
//	containerPort: 80
//	hostPort: 8000
//	listenAddress: 127.0.0.1
//	protocol: TCP
# `type PortMapping struct {`
	// Port within the container.
	ContainerPort int32 `yaml:"containerPort,omitempty" json:"containerPort,omitempty"`
	// Port on the host.
	//
	// If unset, a random port will be selected.
	//
	// NOTE: if you set the special value of `-1` then the node backend
	// (docker, podman...) will be left to pick the port instead.
	// This is potentially useful for remote hosts, BUT it means when the container
	// is restarted it will be randomized. Leave this unset to allow kind to pick it.
	HostPort int32 `yaml:"hostPort,omitempty" json:"hostPort,omitempty"`
	// TODO: add protocol (tcp/udp) and port-ranges
	ListenAddress string `yaml:"listenAddress,omitempty" json:"listenAddress,omitempty"`
	// Protocol (TCP/UDP/SCTP)
	Protocol PortMappingProtocol `yaml:"protocol,omitempty" json:"protocol,omitempty"`
}

// MountPropagation represents an "enum" for mount propagation options,
// see also Mount.
# `type MountPropagation string`

const (
	// MountPropagationNone specifies that no mount propagation
	// ("private" in Linux terminology).
	MountPropagationNone MountPropagation = "None"
	// MountPropagationHostToContainer specifies that mounts get propagated
	// from the host to the container ("rslave" in Linux).
	MountPropagationHostToContainer MountPropagation = "HostToContainer"
	// MountPropagationBidirectional specifies that mounts get propagated from
	// the host to the container and from the container to the host
	// ("rshared" in Linux).
	MountPropagationBidirectional MountPropagation = "Bidirectional"
)

// PortMappingProtocol represents an "enum" for port mapping protocol options,
// see also PortMapping.
# `type PortMappingProtocol string`

const (
	// PortMappingProtocolTCP specifies TCP protocol
	PortMappingProtocolTCP PortMappingProtocol = "TCP"
	// PortMappingProtocolUDP specifies UDP protocol
	PortMappingProtocolUDP PortMappingProtocol = "UDP"
	// PortMappingProtocolSCTP specifies SCTP protocol
	PortMappingProtocolSCTP PortMappingProtocol = "SCTP"
)
