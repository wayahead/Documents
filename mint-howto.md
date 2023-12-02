# Mint Language Howto

## Installation

[Ref](https://mint-lang.com/install)

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

## Test Environment

```
$ git clone https://github.com/mint-lang/mint-ui-website.git
$ cd mint-ui-website
$ mint install
$ mint start
```
