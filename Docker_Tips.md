# Docker Tips/Shortcuts

Tips for using [Docker](https://docker.com/) container/virtualization platform.

## Install Docker on Ubuntu from Official Repository

### Remove Docker from _Ubuntu_ Repositories
```bash
sudo apt-get purge -y docker-engine docker docker.io containerd runc
sudo apt-get autoremove -y --purge docker-engine docker docker.io containerd runc
```

### Install Prerequisite Packages for HTTPS Support
```bash
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
```

### Add GPG Key for Official Docker Repository
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

### Add Official Docker Repository to APT Sources
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
```

### Check to Ensure Use of Docker CE (Community Edition) Repository
```bash
sudo apt-cache policy docker-ce
```
You should see output _similar_ to the following. The most important aspect is that it references the `https://download.docker.com/` repository.
```bash
docker-ce:
  Installed: (none)
  Candidate: 5:20.10.6~3-0~ubuntu-focal
  Version table:
 *** 5:20.10.6~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```

### Install Docker from Repository
```bash
sudo apt-get install -y docker
```

### Ensure that Docker Service Started Successfully
```bash
sudo systemctl status docker
```
You should see output _similar_ to the following. The most important aspect is that it shows that the service is `active (running)`.
```bash
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2021-05-06 14:21:00 CDT; 12s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 746864 (dockerd)
      Tasks: 11
     Memory: 68.2M
     CGroup: /system.slice/docker.service
             └─746864 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

### Add Your User Account to `docker` Group to Run Docker Without Entering Password
```bash
sudo usermod -aG docker ${USER}
```
You will have to logout (not just close your shell/terminal window) to get permissions for the `docker` group. Of course, you can proceed without these permissions, but you'll need to enter your account password whenever you rung the `docker` (or related) command.

As an alternative, you can open a sub-shell with the new `docker` group member with this command:
```bash
exec su -l ${USER}
```
After logging in, run `id -nG` to confirm that you are a member of the `docker` group.

### Verify Docker Installation
```bash
docker run hello-world
```
You should see Docker pull down (or perhaps update, if you had Docker previously installed) the "Hello, World" Docker image and launch it. You'll see some output including the message `Hello from Docker!`, which confirms successfully installation and configuration.

***

## Install Docker Compose Utility
The [Docker Compose](https://docs.docker.com/compose/) tool allows you to build Docker applications made up of multiple containers and services from a single [YAML](https://yaml.org/) configuration file, [`docker-compose.yml'](https://docs.docker.com/compose/compose-file/). Many packaged applications that use Docker require/expect Docker Compose to be installed. To install it, run:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

[Reference](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

[Reference1](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

[Reference3](https://docs.docker.com/engine/install/ubuntu/)

***

## Completely Uninstall and Remove Docker from Ubuntu

**NOTE**: This will delete **all** Docker images, containers, volumes, and user-created configurations on your system.

### Determine which Docker-related packages are installed
```bash
dpkg -l | grep -i docker
```

### Stop the Docker service
```bash
sudo systemctl stop docker
```

### Uninstall all of the Docker-related application packages
```bash
sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce
```

### Delete the Docker Compose and Docker Machine application files
```bash
sudo rm -rf /usr/local/bin/docker-machine /usr/local/bin/docker-machine /etc/bash_completion.d/docker-machine*
```

### Delete the images, containers, and volumes
```bash
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
```

[Reference](https://itectec.com/ubuntu/ubuntu-how-to-completely-uninstall-docker/)
