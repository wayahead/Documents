# Rust Howto

## Install and Uninstall

### Ubuntu 22.04

```
$ sudo apt install curl build-essential gcc make
$ curl https://sh.rustup.rs -sSf | sh
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$ source "$HOME/.cargo/env"
$ cargo --version
$ rustup --version
$ rustc --version
```

```
$ rustup self update
$ rustup update
```

```
$ rustup self uninstall
$ rm -rf ~/.cargo ~/.rustup
$ rm -f ~/.local/share/bash-completion/completions/rustup
```

## Setting up rustup Command Autocomplete

```
$ mkdir -p ~/.local/share/bash-completion/completions/
$ rustup completions bash > ~/.local/share/bash-completion/completions/rustup
$ source ~/.local/share/bash-completion/completions/rustup
$ rustup TAB
```

## Installing Multiple Versions of Rust and Development Tools

```
$ rustup toolchain list
```
