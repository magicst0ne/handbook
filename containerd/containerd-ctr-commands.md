## How to use containerd with ctr

<img src="/Users/stone/OneDrive/应用/typora/containerd/images/containerd-command-line-clients-2000-opt.png" alt="containerd-command-line-clients-2000-opt" style="zoom: 33%;" />

# 1.  Image

## 1.1.  Pull image

```
ctr image pull docker.io/library/nginx:alpine
```
or
```
ctr i pull docker.io/library/nginx:alpine
```

## 1.2.  List image
```shell
ctr image list
```
or
```shell
ctr image ls
```
or
```shell
ctr i ls
```

## 1.3.  Delete image
```shell
ctr image rm docker.io/library/nginx:alpine
```

or

```shell
ctr i rm docker.io/library/nginx:alpine
```

## 1.4.  Mount image dir
```shell
ctr i mount docker.io/library/nginx:alpine /mnt/nginx_alpine/
```
## 1.5.  Unmount image dir
```shell
ctr i unmount /mnt/nginx_alpine/
```
## 1.6.  Export image
use --all-platforms option to fix "ctr: content digest sha256:xxxxxx not found"
```shell
ctr i pull --all-platforms docker.io/library/nginx:alpine
```
```shell
ctr i export --all-platforms nginx.tar docker.io/library/nginx:alpine
```
## 1.7.  Import image
use --all-platforms option to fix "ctr: content digest sha256:xxxxxx not found"
```shell
ctr i import --all-platforms nginx.tar
```
# 2. Container
## 2.1.  Create container

```shell
ctr container create --net-host -t docker.io/library/nginx:alpine nginx_test01
```
or
```shell
ctr c create --net-host -t docker.io/library/nginx:alpine nginx_test01
```

## 2.2.  list container
```shell
ctr container list
```
or
```shell
ctr container ls
```
or
```shell
ctr c ls
```

## 2.3.  Delete container
```shell
ctr container rm nginx_test01
```
or
```shell
ctr c rm nginx_test01
```


## 2.4.  Create container and run
-    --rm                                     remove the container after running
-    --null-io                               send all IO to /dev/null
-    --detach, -d                        detach from the task after it has started execution
-    --net-host                           enable host networking for the container
-    --tty, -t                                 allocate a TTY for the container

```shell
ctr run --net-host -d --rm -t docker.io/library/nginx:alpine nginx_test02
```
# 3. Task
## 3.1.  Stop container
```shell
ctr task kill -9 nginx_test02
```
or
```shell
ctr t kill -9 nginx_test02
```

## 3.2.  Run container

-    --null-io         send all IO to /dev/null
-    --log-uri value   log uri
-    --fifo-dir value  directory used for storing IO FIFOs
-    --pid-file value  file path to write the task's pid
-    --detach, -d      detach from the task after it has started execution

```shell
ctr task start nginx_test02
```
or
```shell
ctr t start nginx_test02
```

## 3.3.  Pause a running container
```shell
ctr task pause nginx_test02
```
or
```shell
ctr t pause nginx_test02
```

## 3.4.  Resume a running container
```shell
ctr task resume nginx_test02
```
or
```shell
ctr t resume nginx_test02
```

## 3.5.  List running container
```shell
ctr task list nginx_test02
```
or
```shell
ctr t list nginx_test02
```
or
```shell
ctr t ls nginx_test02
```

## 3.6.  List processes for running container
```shell
ctr task ps nginx_test02
```
or
```shell
ctr t ps nginx_test02
```

## 3.7.  Get runtime metric
```shell
ctr task metric nginx_test02
```
or
```shell
ctr t metric nginx_test02
```

## 3.8.  Attach to the IO of a running container
```shell
ctr task attach nginx_test02
```
or
```shell
ctr t attach nginx_test02
```

## 3.9.  Run command on running container
--exec-id is required
```shell
ctr task exec --exec-id 0 -t nginx_test02 /bin/sh
```
or
```shell
ctr t exec --exec-id 0 -t nginx_test02 /bin/sh
```
## 3.10.  Delete a running container
```shell
ctr task rm nginx_test02
```
or
```shell
ctr t rm nginx_test02
```
## 4.1.  List namespace
```shell
ctr namespace list
```
or
```shell
ctr ns list
```
or
```shell
ctr ns ls
```
## 4.2.  Create namespace
```shell
ctr namespace create test
```
or
```shell
ctr ns create test
```
or
```shell
ctr ns create test
```

## 4.3.  Delete namespace
```shell
ctr namespace rm test
```
or
```shell
ctr ns rm test
```
or
```shell
ctr ns rm test
```
## 4.4.  Use namespace
-n use namespace
```shell
ctr -n default image ls
```




References:
https://iximiuz.com/en/posts/containerd-command-line-clients/
