# Crystal Setup Howto

## Installation

[Ref](https://crystal-lang.org/install/)

```
# Ubuntu 22.04
# https://crystal-lang.org/install/on_ubuntu
```

### Install Dependencies

```
$ sudo apt update
$ sudo apt install git
$ sudo apt install build-essential pkgconf

$ sudo apt install libc6-dev
$ sudo apt install libevent-dev
$ sudo apt install libpcre2-dev libpcre3-dev
$ sudo apt install libpng-dev
$ sudo apt install zlib1g-dev
$ sudo apt install libssl-dev
$ sudo apt install libxml2-dev
$ sudo apt install libyaml-dev
$ sudo apt install libgmp-dev
$ sudo apt install libz-dev

$ sudo apt install clang-15 llvm-15 lldb-15 lld-15
$ sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-15 15 --slave /usr/bin/clang++ clang++ /usr/bin/clang++-15
$ sudo update-alternatives --config clang
$ sudo update-alternatives --install /usr/bin/lld lld /usr/bin/lld-15 15
$ sudo update-alternatives --config lld
$ sudo update-alternatives --install /usr/bin/lldb lldb /usr/bin/lldb-15 15
$ sudo update-alternatives --config lldb
```

### Install via DEB Repository

```
$ sudo apt update
$ curl -fsSL https://crystal-lang.org/install.sh | sudo bash
```

```
$ sudo apt update
$ echo "deb http://download.opensuse.org/repositories/devel:/languages:/crystal/{REPOSITORY}/ /" | sudo tee /etc/apt/sources.list.d/crystal.list
# Add signing key
$ curl -fsSL https://download.opensuse.org/repositories/devel:/languages:/crystal/{REPOSITORY}/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/crystal.gpg > /dev/null
$ sudo apt update
$ sudo apt install crystal
```

### Install via Linuxbrew

```
# install Homebrew
$ sudo apt update
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
$ export PATH="$PATH:/home/linuxbrew/.linuxbrew/bin"
$ brew update
$ brew install crystal-lang
$ brew install crystal-lang --with-llvm
```

### Install via Snapcraft

```
$ sudo snap install crystal --classic
```

## Configure Vim

[Ref](https://github.com/vim-crystal/vim-crystal)

### `.vimrc`

```
$ nano ~/.vimrc
:set tabstop=2 shiftwidth=2 expandtab
```

### Crystal Syntax Highlight

```
# Vim8
$ mkdir -p ~/.vim/pack/crystal/start
$ cd ~/.vim/pack/crystal/start
$ git clone https://github.com/vim-crystal/vim-crystal.git
```

## Configure Git

```
$ git config --global user.email "kun.liu@outlook.com"
$ git config --global user.name "kun.liu"
```

## Scaffold Project

### Initialize library project

```
$ crystal init lib <proj-name>
```


### Initialize application project

```
$ crystal init app <proj-name>
```

### `CRYSTAL_PATH`

```
$ crystal env CRYSTAL_PATH
$ export CRYSTAL_PATH="$CRYSTAL_PATH:</path/to/lib>"
```
