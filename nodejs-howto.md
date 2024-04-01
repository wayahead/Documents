# NodeJS Howto

## Installation

### Ubunt 22.04

```
$ curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh
$ nano nodesource_setup.sh
$ sudo bash nodesource_setup.sh
$ sudo apt install nodejs
$ node -v
$ npm -v
$ npx -v
```

```
$ sudo apt remove nodejs npm
$ sudo apt purge nodejs npm
```

## Configuration

### List Default

```
$ npm config list
$ npm config ls -l
```

### Configure Registry

```
$ npm config get registry
$ npm config set registry https://registry.npm.taobao.org
```

### Configure Path for Global Modules

```
$ npm root -g
/usr/lib/node_modules
```

```
$ mkdir $HOME/.npm-global
$ npm config get prefix
$ npm config set prefix $HOME/.npm-global
$ vi $HOME/.profile
export PATH=~/.npm-global/bin:$PATH
$ source $HOME/.profile
```

```
$ mkdir $HOME/.npm-global
$ npm config get prefix
$ npm config set prefix $HOME/.npm-global
$ vi $HOME/.bashrc
export NPM_CONFIG_PREFIX=$HOME/.npm-global
$ source $HOME/.bashrc
```
