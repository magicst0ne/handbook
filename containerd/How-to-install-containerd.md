## How to use install containerd

1. Download containerd all in one tar package

```shell
curl -LJO https://github.com/containerd/containerd/releases/download/v1.7.0/cri-containerd-cni-1.7.0-linux-amd64.tar.gz
```



2. Unpack files create config file and start service

```shell
tar -C /  -xzf cri-containerd-cni-1.7.0-linux-amd64.tar.gz
mkdir -p /etc/containerd
containerd config default > /etc/containerd/config.toml

systemctl enable containerd
systemctl start containerd
```

3. Test containerd can work 

```shell
ctr version
```
4. Pull a image
```shell
ctr i pull docker.io/library/nginx:alpine
```

5. Create and run container
```shell
ctr run --net-host -d --rm -t docker.io/library/nginx:alpine nginx_test
```

