# Github Sync Howto

## Install Github CLI on Ubuntu

[Reference](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)

```
$ type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
$ curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | \
sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg && \
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg && \
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | \
sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null && \
sudo apt update && sudo apt install gh -y
```

## Generate Access Token

[Link](https://github.com/settings/tokens)

## Sync Local to Remote

```
$ git remote add origin https://github.com/wayahead/<remote-repository>.git
$ cd <local-repository> && git push -u origin master
```
