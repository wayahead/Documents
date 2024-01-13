# Chromium Build Howto

## References

[Get the code: Checkout, Build, & Run Chromium](https://www.chromium.org/developers/how-tos/get-the-code/)
[Checking out and building Chromium on Linux](https://chromium.googlesource.com/chromium/src/+/main/docs/linux/build_instructions.md)
[Building old revisions](https://chromium.googlesource.com/chromium/src/+/main/docs/building_old_revisions.md)

## System requirements

```
1. Disable IPv6 if behind GFW
2. Setup Docker
3. Docuble check curl, git, lsb_release, python3 and sudo
```

## Install `depot_tools`

```
$ cd ~/workspace
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
$ vi ~/.bashrc
export PATH="$HOME/workspace/depot_tools:$PATH"
$ source ~/.bashrc
$ which fetch
~/workapce/depot_tools/fetch
```

## Get the code

```
$ cd workspace
$ mkdir chromium
$ cd chromium
$ fetch --nohooks chromium
```

## Install additional build dependencies

```
$ cd workspace
$ cd chromium/src
$ ./build/install-build-deps.sh
```

## Sync a release tag

```
$ cd workspace
$ cd chromium/src

# Make sure you have all the release tag information in your checkout.
$ git fetch --tags

# Checkout whatever version you need (known versions can be seen with
# 'git show-ref --tags')
$ git checkout tags/74.0.3729.131
$ gclient sync --with_branch_heads --with_tags
```

## Get compatible `depot_tools`

```
# Get date of current revision:
$ cd workspace
$ cd chrome/src
$ COMMIT_DATE=$(git log -n 1 --pretty=format:%ci)

# Check out depot_tools revision from the same time:
$ cd workspace
$ cd depot_tools
$ git checkout $(git rev-list -n 1 --before="$COMMIT_DATE" main)
$ export DEPOT_TOOLS_UPDATE=0
```

## Clean your working directory

```
$ cd workspace
$ cd chromium/src
$ git clean -ffd
```

## Sync dependencies

```
$ cd workspace
$ cd chromium/src
$ gclient sync -D --force --reset
$ gclient sync -D --force --reset --with_branch_heads
```

```
Warning: gclient sync may overwrite the URL of your origin remote if it encounters problems. You'll notice this when Git starts thinking everything is “untracked” or “deleted”. If this happens, fix and fetch the remote before continuing:

$ cd workspace
$ cd chromium/src
$ git remote get-url origin https://chromium.googlesource.com/chromium/deps/opus.git
$ git remote set-url origin https://chromium.googlesource.com/chromium/src.git
$ git fetch origin
```

## Run the hooks

```
$ cd workspace
$ cd chromium/src
$ gclient runhooks
```

## Setup the build

```
$ cd workspace
$ cd chromium/src
$ gn args out/Default
is_debug = false
is_component_build = true
symbol_level = 0
blink_symbol_level=0
v8_symbol_level=0

$ gn gen out/Default
```

## Build chromium

```
$ cd workspace
$ cd chromium/src
$ autoninja -C out/Default chrome
```

## Run chromium

```
$ cd workspace
$ cd chromium/src
$ out/Default/chrome
```
