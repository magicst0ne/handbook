## 1.Install bash git gmake:
```

pkg install bash git gmake

```

## 2.Install go:

```
setenv GO_VER 1.19.3
setenv DOWNLOAD_URL https://go.dev/dl
echo "Downloading ${DOWNLOAD_URL}/go${GO_VER}.linux-amd64.tar.gz"
curl -sL ${DOWNLOAD_URL}/go${GO_VER}.linux-amd64.tar.gz -o /tmp/go${GO_VER}.linux-amd64.tar.gz

tar -C /usr/local -xf /tmp/go${GO_VER}.linux-amd64.tar.gz
    
echo "setenv PATH $PATH:/usr/local/go/bin" >> ~/.profile
setenv PATH $PATH:/usr/local/go/bin
go version

```


## 3.Build containerd from source
```

# Check out containerd source
git clone https://github.com/containerd/containerd.git
cd containerd
# Minimum required containerd version is v1.6.7
# Find the latest with `git tag -l | tail`
git checkout v1.6.8
# Use gnu make!
gmake install

```

## 4.Build runj from source

```

git clone https://github.com/samuelkarp/runj.git
cd runj
make && make install


```

## 5.Enable Linux emulation

```

kldload linux
# To load the Linux module at boot, run
# echo 'linux_load="YES"' >> /boot/loader.conf
service linux onestart
# To enable Linux emulation at boot, run
# echo 'linux_enable="YES"' >> /etc/rc.conf


```

## 6.Configuring containerd

```

mkdir /var/lib/containerd/io.containerd.snapshotter.v1.zfs
zfs create -o mountpoint=/var/lib/containerd/io.containerd.snapshotter.v1.zfs zroot/containerd

containerd config default | sed '/io.containerd.snapshotter.v1.zfs/{n;s/root_path\ \= \"\"/root_path\ \= \"\/var\/lib\/containerd\/io.containerd.snapshotter.v1.zfs\"/;}'

    
```

## 7.Running containerd

```
cat > /usr/local/etc/rc.d/containerd << EOF

#!/bin/sh

. /etc/rc.subr

name="containerd"
rcvar=containerd_enable
load_rc_config ${name}

: ${containerd_enable="NO"}
: ${containerd_conf="/usr/local/etc/containerd/config.toml"}

pidfile="/var/run/${name}.pid"

command="/usr/sbin/daemon"
command_args="-r -S -t ${name} -T ${name} -P ${pidfile} /usr/local/bin/${name} -c ${containerd_conf}"

run_rc_command "$1"

EOF

service containerd onestart
echo 'containerd_enable="YES"' >> /etc/rc.conf
    
ctr version

    
```

## 8.Run a Linux container!

```

# Pull the image
ctr image pull --platform=linux docker.io/library/alpine:latest
#
# And ....
ctr run --rm --tty --runtime wtf.sbk.runj.v1 --snapshotter zfs --platform linux docker.io/library/alpine:latest mylinuxcontainer uname -a


```
