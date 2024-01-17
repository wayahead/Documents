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

## Get and Run Docker Image

```
# pull latest version of headless-shell
$ sudo docker pull chromedp/headless-shell:latest

# pull specific tagged version of headless-shell
$ sudo docker pull chromedp/headless-shell:74.0.3717.1

# run
$ sudo docker run -d -p 9222:9222 --rm --name headless-shell chromedp/headless-shell

# if headless-shell is crashing with a BUS_ADRERR error, pass a larger shm-size:
$ sudo docker run -d -p 9222:9222 --rm --name headless-shell --shm-size 2G chromedp/headless-shell

# run with entrypoint
$ sudo docker run --restart always -d -p 9222:9222 --name headless-shell --shm-size 2G --entrypoint "/headless-shell/headless-shell" bewise/headless-shell:1.1 --no-sandbox --use-gl=angle --use-angle=swiftshader --remote-debugging-address=0.0.0.0 --remote-debugging-port=9222 --user-agent="Mozilla/5.0 (Linux; Android 13; SM-S908B) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Mobile Safari/537.36" --disable-web-security --allow-running-insecure-content

$ sudo docker stats
$ sudo docker stop headless-shell
$ sudo docker start headless-shell
```
## Stop and Delete docker container

### List all containers

```
$ sudo docker ps -a
```

### Stop container

```
$ sudo docker stop <container_id>
```

### Delete container

```
$ sudo docker rm <container_id>
$ sudo docker rm --force <container_id>
```

## Customize Docker Image

```
For example, in order to make chromedp/headless-shell support CJK:

1. Download chromedp/headless-shell:latest image
2. Write customized Dockerfile
3. Build new image based on the customized Dockerfile
4. Run the new image
```

### List installed images

```
$ sudo docker images
REPOSITORY                TAG       IMAGE ID       CREATED         SIZE
chromedp/headless-shell   latest    70b09876e4dd   6 months ago    285MB
hello-world               latest    d2c94e258dcb   8 months ago    13.3kB
```

### Customize Dockerfile

```
# 1.1
$ vi Dockerfile
FROM chromedp/headless-shell:latest

WORKDIR /usr/share/fonts

RUN apt-get update -y \
    && apt-get install -y fonts-indic \
    && apt-get install -y fonts-noto \
    && apt-get install -y fonts-noto-cjk
```

### Build New Docker Image

```
$ sudo docker build -t bewise/headless-shell:1.1 .
$ sudo docker images
REPOSITORY                TAG       IMAGE ID       CREATED          SIZE
bewise/headless-shell     1.1       a19f419b2c0b   10 seconds ago   1.11GB
chromedp/headless-shell   latest    70b09876e4dd   6 months ago     285MB
hello-world               latest    d2c94e258dcb   8 months ago     13.3kB
```

### Run New Docker Image

```
$ sudo docker run --restart always -d -p 9222:9222 --name headless-shell --shm-size 2G --entrypoint "/headless-shell/headless-shell" bewise/headless-shell:1.1 --no-sandbox --use-gl=angle --use-angle=swiftshader --remote-debugging-address=0.0.0.0 --remote-debugging-port=9222 --user-agent="Mozilla/5.0 (Linux; Android 13; SM-S908B) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Mobile Safari/537.36" --disable-web-security --allow-running-insecure-content
$ sudo docker stats
$ sudo docker stop headless-shell
$ sudo docker start headless-shell
```

### Delete Docker Image

```
$ sudo docker rm bewise/headless-shell:1.0
```
