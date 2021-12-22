# Docker Tips/Shortcuts

Tips for using [Docker](https://docker.com/) container/virtualization platform.

## Docker Terminology
- [Dockerfile](https://docs.docker.com/engine/reference/builder/): A specification (plan) for building a Docker _image_.
- Image: A template for Docker containers that has a specific purpose.
- Container: An instance (either running or finished) of an _image_ for running process/application.

## Common Commands
| Command | Action   |
|:--------|:---------|
| `docker run -it ubuntu bash` | Run (start) Docker _image_ named `ubuntu`, downloading it, if necessary, and execute `bash` application in _container_ in interactive (`-it`) mode. |
| `docker ps -a` | List **all** (`-a`) of the _containers_, running or not. |
| `docker start --attach container_name` | Launch existing ("reuse") _container_ with name container_name and show output (`--attach`). |
| `docker stop container_name` | Stop running container named container_name. |
| `docker rm -f container_name ` | Remove/delete container named container_name and force (`-f`) delete if container is still running. |
| `docker image ls` | List the Docker _images_ downloaded to the system. |

## Basic Docker Examples

### Publish _local_ directory with Docker Nginx image
In this example, our HTML, CSS, etc. files are in the `/var/local/html` directory.
```
docker run -v /var/local/html:/usr/share/nginx/html:ro -p 8080:80 -d nginx
```
Here's what the various parts of the command mean:
- `-v /var/local/html:/usr/share/nginx/html:ro`: Maps the local `/var/local/html` directory with our web page resources to `/usr/share/nginx/html` in the container. Specifying `ro` tells Dockers to mount it in read-only mode, meaning that the container can't/won't make any changes.
- `-p 8080:80`: Maps network service port 80 in the _container_ to port 8080 on the host system (the system running the Docker instance). This means that you would access the web site at port 8080 from the host (e.g., http://127.0.0.1:8080/).
- `-d`: Detaches the container from the command line session. In other words, the container continues running in the background.
- `nginx`: The name of the Docker _image_ to use for the _container_.



[Reference1](https://stackify.com/docker-tutorial/)

### Build a containerized Node.JS/Express.JS application with PostgreSQL using Docker Compose

[Reference](https://dev.to/alexeagleson/docker-for-javascript-developers-41me)
[GitHub Repository](https://github.com/alexeagleson/docker-node-postgres-template)

## Install Docker on Ubuntu from Official Repository

### Remove Any Version of Docker Installed from _Ubuntu_ Repositories
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
sudo apt-get install -y docker-ce
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

### Add Your User Account to _docker_ Group to Run Docker Without Entering Password
```bash
sudo usermod -aG docker ${USER}
```
You will have to logout (not just _close_ your shell/terminal window) to get permissions for the `docker` group. Of course, you can proceed without these permissions, but you'll need to enter your account password whenever you rung the `docker` (or related) command.

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

[Reference1](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)  
[Reference2](https://docs.docker.com/engine/install/ubuntu/)

***

## Install Docker Compose Utility
The [Docker Compose](https://docs.docker.com/compose/) tool allows you to build Docker applications made up of multiple containers and services from a single [YAML](https://yaml.org/) configuration file, [`docker-compose.yml`](https://docs.docker.com/compose/compose-file/). Many packaged applications that use Docker require/expect Docker Compose to be installed. To install it, run:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

[Reference](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

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
