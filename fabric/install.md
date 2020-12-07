# Hyperledger Fabric installation

> This instructions are for x86_64/amd64 architectures of Ubuntu (>= 16.04) and Debian (>= 9)

## Docker installation

As root:
```bash
apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/$(lsb_release -is | tr [:upper:] [:lower:])/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(lsb_release -is | tr [:upper:] [:lower:]) $(lsb_release -cs) stable"
apt update
apt install -y docker-ce docker-ce-cli containerd.io
```

> More info at [Install Docker Engine](https://docs.docker.com/engine/install)

## Docker post-installation

As root:
```bash
groupadd docker
usermod -aG docker <YOUR_USER>
```

As user:
```bash
newgrp docker
docker version
docker run hello-world
```

> More info at [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall)

## Docker compose installation

As root:
```bash
curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

As user:
```bash
docker-compose -v
```

> Take into account that can exist a newer version than 1.27.4 (check [here](https://docs.docker.com/compose/release-notes))

> More info at [Install Docker Compose](https://docs.docker.com/compose/install)


## Golang Installation

As root:
```bash
cd /tmp
curl -O https://dl.google.com/go/go1.15.6.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.15.6.linux-amd64.tar.gz
```

As user:
```bash
mkdir $HOME/go
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go' >> ~/.profile
source ~/.profile
go version
```

> Take into account that can exist a newer version than 1.15.6 (check [here](https://golang.org/dl))

> More info at [GO Download and install](https://golang.org/doc/install)

## Hyperledger Fabric Installation

As user:
```bash
cd $HOME
curl -sSL https://bit.ly/2ysbOFE | bash -s
echo 'export PATH=$PATH:$HOME/fabric-samples/bin' >> ~/.profile
source ~/.profile
```

> **Notes:**
> * You can specify the fabric and fabric-ca versions with `curl -sSL https://bit.ly/2ysbOFE | bash -s -- <fabric_version> <fabric-ca_version>`
> > * The downloaded script clone *fabric-samples* and download the binaries under the *bin* directory

> More info at [Install Samples, Binaries, and Docker Images](https://hyperledger-fabric.readthedocs.io/en/latest/install.html)
