## DockerLess Containers

### Cotainer Runtime

- runtime-spec
- image-spec
- distribution-spec

#### Container Images

- Skopeo ( Image Spec for Container)

--- Fedora Installation of Skopeo ----

```
yum install skopeo
```

Skopeo is used for building image spec.

#### Container Bundles

- umoci ( Build Runtime Spec)

```
umoci 
```

Umoci is used for building `runtime-spec` from `image-spec`. 

#### Container Runtime

- runc ( build Container from runtime spec)

using runc is use for  building container. under `rootfs` directory creted by `umoci`

```
runc run test
```

alternatives to run is `crun`

## Dockerless Tools

Fully Featured tools

```
buildah
```

Skopeo provides only copy & inspect images. Doesn't do build, pull, push , rmi

but buildah is a fully featured - which supprots build, pull, push ,rmi, copy, inspect

#### Dockerfile vs ContainerFile

#### Dockerless tools

- buildah
- podman

Both provides deamonless & rootless 

#### CRI 

CRI is the intergace for K8s to support multiple containers, such as rkt, docker, containerd, cri-o




