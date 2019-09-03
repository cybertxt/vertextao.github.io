---

layout: post
title: Git Skills
date: 2017-05-09 15:50:00 +0800

---

# Git Skills

## constant pretty log configurations

add the following alias to ~/.gitconfig

```
[alias]
        lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
        lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
        lg = !"git lg1"
```

## pretty log for handy usage

```
git log --all --decorate --oneline --graph
```

short for memorizing "a dog" -- **a**ll, **d**ecorate, **o**neline, **g**raph
![git-a-dog](/res/git-a-dog.jpg)

and you can config `adog` to alias
```
git config --global alias.adog "log --all --decorate --oneline --graph"
```

ref: http://stackoverflow.com/a/35075021/1582229
