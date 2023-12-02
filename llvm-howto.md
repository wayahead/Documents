# LLVM Howto

## Install on Ubuntu 22.04

### Use LLVM Repository

```
$ wget https://apt.llvm.org/llvm.sh
$ chmod +x llvm.sh
$ sudo ./llvm.sh <version number>
$ sudo ./llvm.sh 16

$ sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-16 16
$ sudo update-alternatives --config clang
$ sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-16 16
$ sudo update-alternatives --config clang++
$ sudo update-alternatives --install /usr/bin/lld lld /usr/bin/lld-16 16
$ sudo update-alternatives --config lld
$ sudo update-alternatives --install /usr/bin/lldb lldb /usr/bin/lldb-16 16
$ sudo update-alternatives --config lldb
```
