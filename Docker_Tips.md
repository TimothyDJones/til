# Docker Tips/Shortcuts

Tips for using [Docker](https://docker.com/) container/virtualization platform.

## Docker Terminology
- [**Dockerfile**](https://docs.docker.com/engine/reference/builder/): A specification (plan) for building a Docker _image_.
- **Image**: A template for Docker containers that has a specific purpose. Basically, the image provides the custom **file system** structure required for the application.
- **Container**: An instance (either running or finished) of an _image_ for running process/application. Containers are isolated from other processes on the _host_ machine using Linux [kernel namespaces](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504) and [cgroups](https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work/), which have been part of Linux for a long time.

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

## Run Docker on Windows 10 *without* Docker Desktop
Docker changed their licensing model recently and many developers must now use the new subscription-based model to use Docker Desktop tool. However, frequently, developers only use the command-line tools in their day-to-day work and so the Docker Desktop tool is overkill for their needs anyway. 

This article explains how to run Docker on Windows in a very simple command-line-only configuration. And it works on even relatively old Windows 10 systems, including those with Windows Subsystem for Linux (WSL) 1. Essentially, we will be running the Docker daemon (or "service", if you prefer) in a Virtualbox _or_ Hyper-V virtual machine (VM) and accessing it from our Windows 10 host machine in WSL or PowerShell (or Windows Command Prompt).

### What You Need
To use this configuration, we will use the following environment.
- Windows 10
- WSL 1 or WSL 2 with Ubuntu 20.04
- Virtualbox 6.2 with Ubuntu 20.04 guest **or** Hyper-V with Ubuntu 20.04 guest
As noted, we can use either Virtualbox or Hyper-V for the virtualization platform. This allows Windows 10 Home users to use this process, even though they do not Hyper-V support on their platform.

### Install Ubuntu 20.04 in WSL
Install Ubuntu 20.04 (or 18.04) in WSL according to the standard installation process. Here is a brief outline of the process.
1. To install WSL, open PowerShell (or Windows Command Prompt) and run
```bash
wsl.exe --install
```
2. By default, the WSL installation will install Ubuntu. You can also check for other available distributions and versions:
```bash
wsl.exe --list --online
```
3. Then, you can install one of the available distributions from this list:
```bash
wsl.exe --install -d <distroname>
```
where `<distroname>` is the name from the earlier list, such as `ubuntu2004`.

### Install Virtualbox or Hyper-V Virtualization Platform
As explained earlier, you can use *either* [Virtualbox](https://www.virtualbox.org/) or [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/) virtualization platform, depending on your preference and what is supported in your environment. Simply follow the standard installation process for the selected tool. (Note that you **cannot** use _both_ Virtualbox and Hyper-V simultaneously due to the Hyper-V architecture.)
- [Installing Virtualbox on Windows Hosts](https://www.virtualbox.org/manual/ch02.html#installation_windows)
- [Install Hyper-V on Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

### Install Ubuntu Linux Guest in Virtualbox or Hyper-V
After installing Virtualbox or Hyper-V, you will need to install Ubuntu (or other Debian-based) Linux as a guest operating system (OS) on the virtualization platform. You can install a standard GUI version or a very minimal command-line only version. This guest OS will only run Docker daemon (or "service") process, so the OS GUI is entirely optional.

Follow the standard installation process for either Virtualbox or Hyper-V for installing guest OSes. Make sure to allocate at least **2GB** of RAM to the guest.
- [Creating Your First Virtualbox Virtual Machine](https://www.virtualbox.org/manual/ch01.html#gui-createvm)
- [Create Virtual Machine with Hyper-V on Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/create-virtual-machine)

### Install Docker Daemon on Ubuntu Linux Guest in Virtualbox or Hyper-V
Once you've successfully installed Ubuntu as a guest OS in Virtualbox or Hyper-V, in that Ubuntu instance (and **not** the WSL instance _yet_!), install the Docker daemon application. Again, we will be running the Docker daemon in the _guest_ virtual machine (VM) and accessing it from our Windows 10 _host_ machine.

Update the Ubuntu packages.
```bash
sudo apt update
sudo apt upgrade -yy
```

Remove any existing Docker installation from standard Ubuntu repositories. (If you just installed Ubuntu guest OS, it's unlikely that they are installed, but it doesn't hurt to check.)
```bash
sudo apt remove docker docker-engine docker.io containerd runc -y
```

Configure the official Docker repository and install Docker from it.
```bash
source /etc/os-release
curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo apt-key add -
echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

The installation process creates a `docker` group in Linux. We need to add our user to that group to allow us to run Docker commands without using `sudo`.
```bash
sudo usermod -a -G docker ${USER}
```
You must close the terminal window and open a new one (or log out and log back in, if you are using a console/command-prompt only VM) to get a session in which your user belongs to the `docker` group. To confirm, run the `groups` command and ensure that `docker` is included in the list (it will probably be the last one).

To verify that everything is working properly, while still in our Linux guest VM, run `docker info`. You should see some output divided up into `Client` and `Server` sections. See the [`docker info`](https://docs.docker.com/engine/reference/commandline/info/) command documentation for details and examples.

### Configure Docker Daemon for External Access on Ubuntu Linux Guest in Virtualbox or Hyper-V
Now that the Docker daemon is installed and working, we need to make it accessible outside of the Virtualbox or Hyper-V guest OS. For our case, to simplify things, we will configure it **without** encryption. Obviously, this involves some risk, but presumably, we will only be accessing from within the same machine. You can learn more about this in the [Docker security documentation](https://docs.docker.com/engine/security/#docker-daemon-attack-surface).

Create a [systemd](https://en.wikipedia.org/wiki/Systemd) service directory for our configuration and create the daemon (service) configuration file.
```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo nano /etc/systemd/system/docker.service.d/options.conf
```

In `options.conf` add the following lines and save the file. Note that there indeed _two_ lines starting with `ExecStart=`.
```bash
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H unix:// -H tcp://0.0.0.0:2375
```

Refresh the `systemd` configuration and restart Docker.
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

Presumably, you won't receive any errors on restart. In any case, you can check that the Docker daemon restarted (is running) by running `sudo systemctl status docker`, if you like.

Basically, this configuration allows local connections from within the Virtualbox or Hyper-V guest OS VM via `-H unix://` and from **any** external client over TCP on port 2375 via `-H tcp://0.0.0.0:2375`.

### Determine the IP Address of Ubuntu Linux Guest in Virtualbox or Hyper-V
The final step involving the Ubuntu Linux guest OS in Virtualbox or Hyper-V is to determine its IPv4 address. We need this IP address to use on the host (WSL or PowerShell) to connect to the Docker daemon remotely.





### References
[Use Docker for Windows in WSL1](https://pscheit.medium.com/use-docker-for-windows-in-wsl-8fc96ece67c8)  
[How to run docker on Windows without Docker Desktop](https://dev.to/_nicolas_louis_/how-to-run-docker-on-windows-without-docker-desktop-hik)  
[Setting Up Docker for Windows and WSL to Work Flawlessly](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)  
[Docker Tip #73: Connecting to a Remote Docker Daemon](https://nickjanetakis.com/blog/docker-tip-73-connecting-to-a-remote-docker-daemon)