# images/base

* goal
  * 👀sources -- for -- building the `kind` base "node" image👀

## how to build the image?

* `make quick`

## Maintenance

* [Dockerfile](./Dockerfile)
  * support
    * running 
      * systemd
      * nested containers
      * Kubernetes

* [logic / interacts -- with -- this image](./../../pkg/cluster)

## Alternate Sources

* ⚠️if you use the [Dockerfile](Dockerfile) & DIFFERENT build arguments (DIFFERENT base image OR application version) -> you may encounter bugs ⚠️
  * Reason: 🧠 Kind FREQUENTLY picks up NEW releases -- of -- DEPENDENT projects (_Examples:_ containerd, runc, cni, and crictl) 🧠 

## Design

* [here](/site/content/docs/design/base-image.md)
