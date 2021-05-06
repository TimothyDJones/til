# Docker Tips/Shortcuts

Tips for using [Docker](https://docker.com/) container/virtualization platform.

## Install Docker on Ubuntu from Official Repositories

### Remove Docker from Ubuntu Repositories
```bash
sudo apt-get purge -y docker-engine docker docker.io
sudo apt-get autoremove -y --purge docker-engine docker docker.io
```


[Reference1](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
[Reference2](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

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
