# Git Howto

## Configure Commit Parameters

```
$ git config --global user.email "kun.liu@outlook.com"
$ git config --global user.name "kun.liu"
```

## Update Local Repository

```
$ git status
$ git pull
$ git status
```

## Checkout Tag

```
$ git fetch --all --tags
$ git checkout tags/<tag> -b <tag>-branch
```

## Get all remote branches

```
$ git pull --all
```

## Branch

### Create and Switch to Branch

```
$ git branch <dev>
$ git checkout <dev>
```

or 

```
$ git checkout -b <dev>
```

### Check Current Branch 

```
$ git branch
```

### List All Branches 

```
$ git branch -a
```

### Commit to Branch

```
$ git checkout -b <dev>
$ git add <updated-file>
$ git commit -m "<message>"
```

### Merge to Master Branch

```
$ git checkout -b <dev>
$ git add <updated-file>
$ git commit -m "<message>"
$ git checkout master
$ git merge <dev>
```

* merge master to saleslead

```
$ git checkout saleslead
$ git branch
$ git merge --no-ff --no-commit master
```

### Delete Branch

```
$ git branch -d <dev>
```

## Submodule

### Add Submodule

```
$ git submodule add <git-repository-url>
```

### Pull Submodule

```
$ git submodule update --init -- <local-path-ti-submodule>
```
