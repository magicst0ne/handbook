## How to Run a Nginx Container

1. **Pull a nginx container**

```shell
ctr i pull docker.io/library/nginx:alpine
```

2. **Create config dir**

```shell
mkdir -p /etc/container-conf/nginx-test01
```

3. **Edit nginx config file** 

```shell
cat > /etc/container-conf/nginx-test01/default.conf <<EOF
server {
        listen       8081;
        server_name  localhost;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
    }
EOF

```

4. **Create and Run a nginx container**

```shell
ctr run --net-host -d --rm -t --mount type=bind,src=/etc/container-conf/nginx-test01,dst=/etc/nginx/conf.d,options=rbind:ro docker.io/library/nginx:alpine nginx-test01
```

5. **Test** 

```shell
curl http://127.0.0.1:8081
```

6. **Enable ip forwarding** 

```shell
echo 1 > /proc/sys/net/ipv4/ip_forward
```

7. **Set port forward on firewall** 

```shell
iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-port 8081
```

8. **Test on anther computer** 

```shell
curl http://x.x.x.x:8080
```
