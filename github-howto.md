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

```
$ gh auth login
? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations on this host? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Paste an authentication token
Tip: you can generate a Personal Access Token here https://github.com/settings/tokens
The minimum required scopes are 'repo', 'read:org', 'workflow'.
? Paste your authentication token: ****************************************
- gh config set -h github.com git_protocol https
✓ Configured git protocol
! Authentication credentials saved in plain text
✓ Logged in as wayahead
```

## Sync Local to Remote

```
$ git remote add origin https://github.com/wayahead/<remote-repository>.git
$ cd <local-repository> && git push -u origin master
```
