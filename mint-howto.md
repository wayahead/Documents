# Mint Language Howto

## Installation

[Reference](https://mint-lang.com/install)

### Install from source code

```
$ git clone https://github.com/mint-lang/mint.git
$ cd mint
$ shards install
$ shards build --progress --release --no-debug
$ sudo mv ./bin/mint /usr/local/bin/mint
```

### Install from binary

```
$ wget --no-verbose -O mint https://mint-lang.s3-eu-west-1.amazonaws.com/mint-master-linux
$ chmod +x ./mint
$ sudo mv ./mint /usr/local/bin/mint
```

### Install by Linuxbrew (Not Recommended)

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
$ brew update

# MacOS
$ xcode-select --install

# If installed mint before
$ brew untap homebrew-community/mint

$ brew tap mint-lang/mint-lang
$ brew install mint-lang
```

### Use docker

```
$ git clone https://github.com/mint-lang/mint-docker.git
$ cd mint-docker
$ docker-compose up
```

## Test Environment

```
$ git clone https://github.com/mint-lang/mint-ui-website.git
$ cd mint-ui-website
$ mint install
$ mint start
```

## Configure Vim

[Ref](https://github.com/IrenejMarc/vim-mint)

### `.vimrc`

```
$ nano ~/.vimrc
:set tabstop=2 shiftwidth=2 expandtab
```

### Mint Syntax Highlight

```
# Vim8
$ mkdir -p ~/.vim/pack/crystal/start
$ cd ~/.vim/pack/crystal/start
$ git clone https://github.com/IrenejMarc/vim-mint.git
```
