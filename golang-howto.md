# Golang Setup Howto

## Configure Vim

```
$ git clone https://github.com/fatih/vim-go.git ~/.vim/pack/golang/start/vim-go
$ vim ~/.vimrc
:set tabstop=2 shiftwidth=2 expandtab
:set nocompatible
```

## Install Golang

### x64

```
$ sudo apt update
$ sudo apt install build-essential git
$ wget --inet4-only https://go.dev/dl/goX.Y.Z.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf goX.Y.Z.linux-amd64.tar.gz
$ sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf goX.Y.Z.linux-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin
$ go version
```

### aarch64

```
$ sudo apt update
$ sudo apt install build-essential git
$ wget --inet4-only https://go.dev/dl/goX.Y.Z.linux-arm64.tar.gz
$ sudo tar -C /usr/local -xzf goX.Y.Z.linux-arm64.tar.gz
$ sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf goX.Y.Z.linux-arm64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin
$ go version
```

## Install Multiple Versions

```
$ go install golang.org/dl/goA.B.C@latest
$ goA.B.C download
$ goA.B.C version
$ goA.B.C env GOROOT
```

```
$ go install golang.org/dl/go1.20.12@latest
$ go1.20.12 download
$ go1.20.12 version
$ go1.20.12 env GOROOT
```

## Configure GOPROXY

```
$ go env GOPROXY
https://proxy.golang.org,direct
$ vi $HOME/.profile
export PATH=$PATH:/usr/local/go/bin
export GOPROXY=https://proxy.golang.com.cn,direct
$ source $HOME/.profile
$ go env
```

## Initialize Project

```
$ mkdir -p <path/to/directory>/project/pinwheel.v2
$ cd <path/to/directory>/project/pinwheel.v2
$ go mod init github.com/wayahead/pinwheel/v2
```

## Build Cross-Platform

```
GOOS: darwin, linux, windows
GOARCH: amd64, arm64
```

### Windows

```
$ GOOS=windows GOARCH=amd64 go build -o <Binary>
```

### MacOS

```
$ GOOS=darwin GOARCH=amd64 go build -o <Binary>
```

### Linux

```
$ GOOS=linux GOARCH=amd64 go build -o <Binary>
```
