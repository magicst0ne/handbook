# How to Set Up a Demo etcd Cluster

## download and install binary

```shell
export ETCD_VER=v3.4.20
export DOWNLOAD_URL=https://github.com/etcd-io/etcd/releases/download
export DOWNLOAD_URL=https://ghproxy.com/${DOWNLOAD_URL}

echo "Downloading "${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz
curl -sL ${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz -o /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
mkdir -p /tmp/etcd-${ETCD_VER}
tar xzf /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz -C /tmp/etcd-${ETCD_VER} --strip-components=1

mkdir -p /usr/local/etcd/{bin,ssl}
mv /tmp/etcd-${ETCD_VER}/etcd* /usr/local/etcd/bin/
chmod +x /usr/local/etcd/bin/*
rm -rf /tmp/etcd-${ETCD_VER}

```

## run a 1 node etcd

## run a 1 node etcd on container

## run a 3 node etcd cluster

## run a 3 node etcd cluster on container
