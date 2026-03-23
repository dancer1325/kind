## images/node

* image / 
  * built programmatically 
    * steps (Reason:🧠performance🧠)
      * `docker run`
      * `docker exec`
      * `docker commit`
    * -- based on -- 
      * [`pkg/build/nodeimage/build.go`](./../../pkg/build/nodeimage/build.go)
      * [`pkg/build/nodeimage/buildcontext.go`](./../../pkg/build/nodeimage/buildcontext.go)

* == [base image](./../base) + 
  * install the Kubernetes packages / binaries
  * place
    * Kubernetes docker images | "/kind/images/*.tar"
    * a file | "/kind/version" / contains the Kubernetes semver

* [design](/site/content/docs/design/node-image.md)
