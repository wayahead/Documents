# Headless Setup Howto

## Get Headless docker image for Chromedp

```
$ sudo docker pull chromedp/headless-shell:latest
$ sudo docker run -d -p 9222:9222 --rm --name headless-shell chromedp/headless-shell
$ sudo docker stats
$ sudo docker stop headless-shell
```

## Install Chinese font package

```
$ sudo su -
# apt-get update -y && \
  apt-get install -y fonts-noto && \
  apt-get install -y fonts-noto-cjk && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/
```
