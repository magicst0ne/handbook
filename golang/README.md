

# How to Set Up a golang env

  
## Download And Install

```shell
#set env vars
export GO_VER=1.19.3
export SHA256SUM=74b9640724fd4e6bb0ed2a1bc44ae813a03f1e72a4c76253e2d5c015494430ba
export DOWNLOAD_URL=https://go.dev/dl

#download
echo "Downloading ${DOWNLOAD_URL}/go${GO_VER}.linux-amd64.tar.gz"
curl -sL ${DOWNLOAD_URL}/go${GO_VER}.linux-amd64.tar.gz -o /tmp/go${GO_VER}.linux-amd64.tar.gz
echo "${SHA256SUM}  sha256sum" 
sha256sum /tmp/go${GO_VER}.linux-amd64.tar.gz

#install
sudo tar -C /usr/local -xvf /tmp/go${GO_VER}.linux-amd64.tar.gz

#setup env
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
export PATH=$PATH:/usr/local/go/bin
go version

```
