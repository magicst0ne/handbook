## Install bash git gmake:
```

    pkg install bash git gmake


```

## Install go:

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


## Build containerd
```

    # Check out containerd source
    git clone https://github.com/containerd/containerd.git
    cd containerd
    # Minimum required containerd version is v1.6.7
    # Find the latest with `git tag -l | tail`
    git checkout v1.6.8
    # Use gnu make!
    gmake install
    # Start containerd
    service containerd onestart
    # For persistence across reboots, add containerd to /etc/rc.conf run     at boot
    # echo 'containerd_enable="YES"' >> /etc/rc.conf
    #
    # This also seems to be necessary
    mkdir /var/lib/containerd/io.containerd.snapshotter.v1.zfs

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

```

## Build runj

```

    git clone https://github.com/samuelkarp/runj.git
    cd runj
    gmake install


```

## Enable Linux emulation

```

    kldload linux
    # To load the Linux module at boot, run
    # echo 'linux_load="YES"' >> /boot/loader.conf
    service linux onestart
    # To enable Linux emulation at boot, run
    # echo 'linux_enable="YES"' >> /etc/rc.conf


```

## Run a Linux container!
```

    ctr version

    
```
