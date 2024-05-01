# Docker Guides

## Install Docker Engine

Install Docker Engine and Docker Compose using the `apt` repository.

```bash
# Uninstall any conflicting packages if any
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Set up Docker's apt repository
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
```bash
# Install the Docker packages
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Verify that the Docker Engine and Compose installation is successful
sudo docker run hello-world
sudo docker compose version
```

You have now successfully installed and started Docker Engine.

### Linux post-installation steps

Create the docker group and add your user.

```bash
sudo groupadd docker
sudo usermod -aG docker $USER

# Verify that you can run docker commands without sudo
docker run hello-world
```

Configure Docker to start on boot with systemd.

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

Use `local` logging driver that performs log rotation by default.

```bash
sudo nano /etc/docker/daemon.json

{
  "log-driver": "local",
  "log-opts": {
    "max-size": "10m"
  }
}

sudo systemctl restart docker.service
docker info --format '{{.LoggingDriver}}'
```

## References

[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
