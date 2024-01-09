# Golang Setup Howto

## Install Golang

### x64

```
$ sudo apt update
$ sudo apt install build-essential git
$ wget --inet4-only https://go.dev/dl/goX.Y.Z.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf goX.Y.Z.linux-amd64.tar.gz
$ sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf goX.Y.Z.linux-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
$ go version
```

### aarch64

```
$ sudo apt update
$ sudo apt install build-essential git
$ wget --inet4-only https://go.dev/dl/goX.Y.Z.linux-arm64.tar.gz
$ sudo tar -C /usr/local -xzf goX.Y.Z.linux-arm64.tar.gz
$ sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf goX.Y.Z.linux-arm64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
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

## Configure Vim

```
$ go install golang.org/x/tools/gopls@latest
$ git clone https://github.com/fatih/vim-go.git ~/.vim/pack/golang/start/vim-go
$ vim ~/.vimrc
:set tabstop=2 shiftwidth=2 expandtab
:set nocompatible
$ vim ~/.profile
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
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

## Modules

### Initialize module

```
# initialize new module in current directory
$ go mod init github.com/wayahead/windmill/v2
# add missing and remove unused modules
$ go mod tidy
# make vendored copy of dependencies
$ go mod vender
```

### Add, Upgrade and Remove dependency

```
$ go help get
# add a dependency
$ go get example.com/pkg
# upgrade or downgrade a package to a specific version
$ go get -u example.com/pkg@latest
$ go get example.com/pkg@v1.2.3
# update all dependencies
$ go get -u
# remove a dependency
$ go get example.com/mod@none
```

### Install binary package

```
$ go install github.com/spf13/cobra-cli@latest
```

## Testing

### Normal Testing

```
$ go test -v
$ go test ./...
$ go test -cover .
$ go test -coverprofile=coverage.out .
$ go tool cover -func=coverage.out
$ go tool cover -html=coverage.out
$ go tool cover -html=coverage.out -o coverage.xhtml
```

### Short Testing

```
$ go test -v -short .
```

```
func Test_Example(t *testing.T) {
  if testing.Short() {
    t.Skip("skipping test in short mode.")
  }
}
```

### Testing not in Parallel

```
$ go test -parallel 1 ./...
```

### Running Specific Tests

```
$ go test -v -run <pattern> ./...
$ go test -v -run Example ./...
```

### Testing Timeout

```
$ go test -timeout 50ms ./...
```

### Failing Fast

```
$ go test -failfast .
```

### Disabling Test Caching

```
$ go test -count=1 ./...
```
