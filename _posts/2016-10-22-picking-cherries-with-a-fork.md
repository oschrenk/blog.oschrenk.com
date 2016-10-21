---
layout: post
title: Picking cherries with a fork
tags:
- git
- cli
---

As you might have noticed I work in `vim` a lot. With neovim gaining traction, the work behind vim has also sped up again. This is great for the community as new feature and improvements are made. Just sometimes though it means that it will break your setup.

When I recently updated my `vim` installation, one of my favourite plugins, [tpope](https://tpo.pe/)’s [vim-vinegar](https://github.com/tpope/vim-vinegar) (a spruced up version of netrw, a directory browser for vim), has stopped working. Whenever I tried to go up a directory, it threw me an error. Sure enough somebody already posted the issue and tpope had a fix ready to go.

The problem was quickly identified and actually fixed in the main project, but I actually don't use the original `vim-vinegar` but a fork, which introduces a tiny change, which I actually agree wit (namely hbeing able to quit netrw by pressing `q`).

But now the fork was not being updated and was broken with current vim.  So what do you do? you create your own fork, and pick whatever changes you need from the other project! You pick and choose! It's like a commit buffet!

Life on the command line and working with Github is a bit easier with [hub](https://hub.github.com/), so I trust that you have that installed (eg. via `brew install hub` or your package manager of choice)

TL;DR GMCLI (too long don't read give me command line)

```shell
# 1. Clone original project
hub clone tpope/vim-vinegar
cd vim-vinegar
# 2. Fork it
hub fork
git remote rename origin upstream
git remote rename oschrenk origin
git branch --set-upstream-to=origin/master
# 3. Cherry pick the commit you want
hub cherry-pick https://github.com/glittershark/vim-vinegar/commit/f5f532
# 4. Push to your fork
git push origin master
# 5. Be happy
```

So what happens here?

1. Clone the main repository with the fixed behaviour. Here `hub ...` is just a shorter way of typing

```shell
git clone git://github.com/tpope/vim-vinegar.git
```

2. Fork the repository. `hub` does a bit of work here and actually creates the fork on Github for you and adds it as a remote. We then go one step further and change the origin to my fork and rename the original origin to upstream.

```shell
cd vim-vinegar
(forking repo on GitHub...)
git remote add oschrenk git://github.com/oschrenk/vim-vinegar.git
git remote rename origin upstream
git remote rename oschrenk origin
git branch --set-upstream-to=origin/master
```

3. Here you still have to put some manual work in and actually browse the commit history of the forks or look at he pull requests to decide which commits you need. But then `hub cherry-pick` just fetches the repo of the fork and cherry-picks that commit on top of your current branch.

```shell
hub cherry-pick https://github.com/glittershark/vim-vinegar/commit/f5f532
git remote add glittershark git://github.com/glittershark/vim-vinegar.git
git fetch glittershark
git cherry-pick f5f532
```

4. And now where are already done. Just push that new commit to make it part of Github. Congratulations you now have your own fork.

```shell
git push origin master
```

5. Don't forget to be happy!

