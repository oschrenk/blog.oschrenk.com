---
layout: post
title: Vim, YouCompleteMe and Python - making it work (on OS X)
---

I'm using [vim](http://www.vim.org/) with the [YouCompleteMe](https://github.com/Valloric/YouCompleteMe) plugin (or YCM in short) [^1].In general I like to be at the edge with my development tools and my operating system for that matter and so I keep changing my configuration and keep upgrading my [homebrew](http://brew.sh/) installation and vim plugins. That means though, that sometimes that things stop working.

So it happened that the YCM plugin failed on me and not only for my [MacVim](https://github.com/macvim-dev/macvim) application but also for my terminal based vim. When I entered insert mode in either one of them I got this error message.

```console
E887: Sorry, this command is disabled, the Python's site module could not be loaded.
```

Say what?

There is a lot of discussion around dependency issues surrounding python, YCM and vim and its seems most of the time (if not all of the time) it is a configuration issue with picking up the right python version and that especially homebrew on OS X makes life harder rather than easier.

The important thing is to **make sure that vim and YCM both are compiled with the same Python version** (and actually same installation directory).

I wanted to start from scratch and removed my installation of vim, macvim and python and reinstalled them.

```console
brew rm vim macvim python
brew install python
brew install vim --override-system-vi
brew install macvim --override-system-vim
sudo brew linkapps macvim
```

That fixed the issue, but let's actually see why it fixed the issue. First let's find out which python  `vim` is actually using.

```
otool -L (which vim) | grep -i python
/usr/local/Frameworks/Python.framework/Versions/2.7/Python (compatibility version 2.7.0, current version 2.7.0)

greadlink -f /usr/local/Frameworks/Python.framework/Versions/2.7
/usr/local/Cellar/python/2.7.10_2/Frameworks/Python.framework/Versions/2.7
```

If you are wondering: `greadlink` is the GNU version of `readlink` on OS X. [^readlink]. And if the command is not working for you. I'm sorry, I use [fish](http://fishshell.com/). But I'm sidetracking.

Obviously vim is using the Python version that was installed using homebrew. That's good.

Looking at the installation script `install.sh` of YCM, it calls in turn the installation procedure of YCM server, which `[calls python-config](https://github.com/Valloric/ycmd/blob/master/build.py#L63)` to choose the correct python installation. So we have to make sure that the `python-config` that is being called belongs to the same installation we used for compiling vim.

```
python-config --prefix
/usr/local/Cellar/python/2.7.10_2/Frameworks/Python.framework/Versions/2.7
```

Good. `python-config` is part of the same installation vim was being installed with. So it works.

Let's quickly check if MacVim is working:

```
otool -L /Applications/MacVim.app/Contents/MacOS/Vim | grep -i python
/usr/local/Frameworks/Python.framework/Versions/2.7/Python (compatibility version 2.7.0, current version 2.7.0)
```

Yes. Everything runs smoothly now and I can enjoy my autocompletion.:w

[^1]: Secretly still hoping that YouCompleteMe will get [ensime support](https://github.com/ensime/ensime-server/issues/1049).
[^readlink]: Unfortunately the BSD version of `readlink` does not support the `-f` flag to get the canonical url. So I use the GNU version. If you don't have `greadlink`, you can install it via `brew install coreutils`. It will bring GNU versions of some standard, some might say core, utilities to OS X, one of which is `readlink`.
