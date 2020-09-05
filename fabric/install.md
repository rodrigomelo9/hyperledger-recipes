# Hyperledger Fabric installation

> For Debian (>= 9) / Ubuntu (>= 16.04)

## Docker installation

As root:
```bash
apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/$(lsb_release -is | tr [:upper:] [:lower:])/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(lsb_release -is | tr [:upper:] [:lower:]) $(lsb_release -cs) stable"
apt update
apt install -y docker-ce docker-ce-cli containerd.io
groupadd docker
usermod -aG docker <YOUR_USER>
```

As user:
```bash
newgrp docker
docker version
docker run hello-world
```

> More info at [Install Docker Engine](https://docs.docker.com/engine/install)

## Docker compose installation

As root:
Install Docker compose
```bash
curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

As user:
```bash
docker-compose -v
```

> More info at [Install Docker Compose](https://docs.docker.com/compose/install)

## Hyperledger Fabric Installation

As user:
```bash
cd $HOME
curl -sSL https://bit.ly/2ysbOFE | bash -s
echo 'export PATH=$PATH:$HOME/fabric-samples/bin' >> ~/.profile
source ~/.profile
```

> More info at [Install Samples, Binaries, and Docker Images](https://hyperledger-fabric.readthedocs.io/en/latest/install.html)

## Golang Installation

As root:
```bash
cd /tmp
curl -O https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
tar xvf go1.12.5.linux-amd64.tar.gz
mv go /usr/local
```

As user:
```bash
mkdir $HOME/go
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go' >> ~/.profile
source ~/.profile
go version
```

> More info at [GO Download and install](https://golang.org/doc/install)
