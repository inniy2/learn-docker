## Hello world
- - - -  
1. Install docker in ubuntu  
[Get Docker CE for ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)  
```bash
# Remove older version:
sudo apt-get remove docker docker-engine docker.io

# Update apt package index:
sudo apt-get update

# Install package to all apt to use a repository over HTTPS:
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Add docker's GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Varify fingerprint
sudo apt-key fingerprint 0EBFCD88

# Add apt repository
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Update the apt package index
sudo apt-get update

# Install latest version of docker
sudo apt-get install docker-ce
```
2. Hello world
```bash
sudo docker run dockerinaction/hello_world
```
