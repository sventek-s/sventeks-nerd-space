---
published: true
---

# Git config Hack

## Background

I have two github users for different things, one as a data hoarder and the other one for my projects. That being the case I have one github config but now when I am pushing to the other github repo the username for my main github account is seen on the other and I am genuinely not happy about that.

## The main Hack

Here comes the hack I found on [stackoverflow](https://stackoverflow.com/a/44036640)

- main gitconfig at home directory.

```md
[include]
  path = ~/.iAmG-r00t.gitconfig

[includeIf "gitdir:~/Documents/github/sventek-s/"]
  path = ~/.sventek.gitconfig

[core]
  editor = /usr/bin/vim
  excludesfile = /home/null/.gitignore

# --- .iAmG-r00t.gitconfig --- #
# default gitconfig
[user]
  name = iAmG-r00t
  email = [REDACTED]

# --- .sventek.gitconfig --- #
# sventek's github config
[user]
  name = sventek-s
  email = [REDACTED]
```

`gitdir` is case-sensitive and `gitdir/i` is case-insensitive.

BOOM, no more mixed users in commits.

## Other hacks

I don't remember where I found this hack, but basically I use SSH keys to manage my git stuff on my pc. While having two accounts I had to find a way to specify what repo's use which key and what not.

SSH config file.

```md
# first user iAmG-r00t
Host github.com
     Hostname github.com
     User git
     PreferredAuthentications publickey
     IdentityFile ~/path/to/ssh/private-key

# second user sventek
Host sventek-github.com
     Hostname sventek-github.com
     User git
     PreferredAuthentications publickey
     IdentityFile ~/path/to/ssh/private-key
```

Then once after cloning or creating a repo for sventek, you need to change the remote url origin.

Example;

- Original from github will look like this; `git@github.com:sventek-s/sventeks-nerd-space.git`
- Edited; `git@sventek-github.com:sventek-s/sventek-s-wiki.git`

command to change remote url origin: `git remote set-url origin git@sventek-github.com:sventek-s/sventek-s-wiki.git`

confirm if change has been made: `git remote show origin`

**[UPDATE]** Added a global gitignore file.
