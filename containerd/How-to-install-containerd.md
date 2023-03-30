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

6. openrc linux start script
```shell
#!/sbin/openrc-run
supervisor=supervise-daemon

name="Container Daemon"
description="Standalone containerd (usually started by Docker)"

extra_started_commands="reload"
description_reload="Reload configuration without exiting"

command="${containerd_command:-/usr/local/bin/containerd}"
command_args="${containerd_opts}"
rc_ulimit="${ulimit_opts:--c unlimited -n 1048576 -u unlimited}"
retry="${signal_retry:-TERM/60/KILL/10}"

log_file="${log_file:-/var/log/${RC_SVCNAME}.log}"
err_file="${err_file:-${log_file}}"
log_mode="${log_mode:-0644}"
log_owner="${log_owner:-root:root}"

supervise_daemon_args="${supervise_daemon_opts:---stderr \"${err_file}\" --stdout \"${log_file}\"}"

depend() {
	need sysfs cgroups
}

start_pre() {
	checkpath -f -m "$log_mode" -o "$log_owner" "$log_file" "$err_file"
}

reload() {
	ebegin "Reloading configuration"
	$supervisor $RC_SVCNAME --signal HUP
	eend $?
}
```


