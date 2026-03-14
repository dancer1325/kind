<!--TODO(bentheelder): fill this in much more thoroughly-->
# images/base

* goal
  * sources -- for -- building the `kind` base "node" image

## how to build the image?

* `make quick`

## Maintenance

TODO: 
This image needs to do a number of unusual things to support running systemd,
nested containers, and Kubernetes. All of what we do and why we do it
is documented inline in the [Dockerfile](./Dockerfile).

If you make any changes to this image, please continue to document exactly
why we do what we do, citing upstream documentation where possible.

See also [`pkg/cluster`](./../../pkg/cluster) for logic that interacts with this image.


## Alternate Sources

Kind frequently picks up new releases of dependent projects including
containerd, runc, cni, and crictl. If you choose to use the provided Dockerfile
but use build arguments to specify a different base image or application version
for dependencies, be aware that you may possibly encounter bugs and undesired
behavior.

## Design

* [base-image](/site/content/docs/design/base-image.md)
