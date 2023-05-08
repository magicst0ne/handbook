## How To Setup Registry

1. **Pull a registry image**

```shell
ctr i pull docker.io/library/registry:2
```

2. **Create data dir**

```shell
mkdir -p /data/docker-registry
```

3. **Create and Run a registry container**

```shell
ctr run --net-host --null-io -d --mount type=bind,src=/data/docker-registry,dst=/var/lib/registry,options=rbind:rw docker.io/library/registry:2 registry
```

or

```shell
ctr c create --net-host --mount type=bind,src=/data/docker-registry,dst=/var/lib/registry,options=rbind:rw docker.io/library/registry:2 registry
ctr t start --null-io -d registry
```

4. **Test** 

```shell
ctr i pull --all-platforms docker.io/library/nginx:alpine

ctr i push 127.0.0.1:5000/nginx:alpine docker.io/library/nginx:alpine
```
