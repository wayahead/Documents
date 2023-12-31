# Docker Setup Howto

[docker-install](https://docs.docker.com/engine/install/ubuntu/)

## Uninstall Old Version

```
$ for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

## Install via APT Repository

### Configure APT Repository

```
$ sudo apt update
$ sudo apt install ca-certificates curl gnupg
$ sudo install -m 0755 -d /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt update
```

### Install Latest

```
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
$ sudo docker run hello-world
```

### Install Specific Version

```
# List the available versions:
$ apt-cache madison docker-ce | awk '{ print $3 }'

5:24.0.0-1~ubuntu.22.04~jammy
5:23.0.6-1~ubuntu.22.04~jammy
...

# Select the desired version and install
$ VERSION_STRING=5:24.0.0-1~ubuntu.22.04~jammy
$ sudo apt install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
$ sudo docker run hello-world
```

## Install via Packages

```
1. Go to https://download.docker.com/linux/ubuntu/dists/.
2. Select your Ubuntu version in the list.
3. Go to pool/stable/ and select the applicable architecture (amd64, or arm64).
4. Download the following deb files for the Docker Engine, CLI, containerd, and Docker Compose packages:
   `containerd.io_<version>_<arch>.deb
    docker-ce_<version>_<arch>.deb
    docker-ce-cli_<version>_<arch>.deb
    docker-buildx-plugin_<version>_<arch>.deb
    docker-compose-plugin_<version>_<arch>.deb`
5. Install the .deb packages. Update the paths in the following example to where you downloaded the Docker packages.
   `$ sudo dpkg -i ./containerd.io_<version>_<arch>.deb \
      ./docker-ce_<version>_<arch>.deb \
      ./docker-ce-cli_<version>_<arch>.deb \
      ./docker-buildx-plugin_<version>_<arch>.deb \
      ./docker-compose-plugin_<version>_<arch>.deb`
6. Verify that the Docker Engine installation is successful by running the hello-world image.
   `$ sudo service docker start
    $ sudo docker run hello-world`
``` 

## Get Docker for Chromedp

```
$ sudo docker pull chromedp/headless-shell:latest
$ sudo docker run -d -p 9222:9222 --rm --name headless-shell chromedp/headless-shell
$ sudo docker stats
$ sudo docker stop headless-shell
```
