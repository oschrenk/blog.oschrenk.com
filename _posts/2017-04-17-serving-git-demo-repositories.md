---
layout: post
title: Serving and hosting git demo repositories
tags:
- git
- cli
---

Next to my day job as a Scala developer, I am, on occasion, giving Git workshops ([shameless plug](https://gittogreat.nl)). While explaining all things Git, I also need to refer to a few Git repositories for demonstration purposes. Normally I hosted these on Github and told the attendees to pull from there.

I saw a few problems with that setup

1. Implicitly endorsing one vendor over another
2. People need to be registered at that vendor

While I believe that Github is doing great work, I don't really want to promote their services over other services such as [Gitlab](https://gitlab.com) or [Bitbucket](https://bitbucket.org) or any other solution for that matter.  I also didn't want to make people signup for on or the other service, if they made other choices for themselves already.

In addition to that, I often want to show or share some impromptu repository, which I don't want to host on any of these services.

It's a distributed system, right? I should be able to host them anywhere right?! So, let's host them on my machine!

I still wanted a nice short and speaking url, though. Tunneling to the rescue.

##  Setup tunnel

**Install [ngrok](https://ngrok.com/)**

```shell
brew cask install ngrok
```

Sign up with [ngrok](https://dashboard.ngrok.com/user/signup)

Get your authorization token [here](https://dashboard.ngrok.com/auth) and add it

```shell
ngrok authtoken <token>
```

**Configuration**

The configuration file is at `$HOME/.ngrok2/ngrok.yml`. You will already find a `token` entry.

Let's define a tunnel:

```
authtoken: <token>
tunnels:
  git:
    proto: http
    addr: 8174
    region: eu
    hostname: git.oschrenk.com
```

To use a custom domain, you need to pay for ngrok.

Have a registered domain, where you manipulate the CNAME entries of subdomains.

Reserve a domain in ngrok [here](https://dashboard.ngrok.com/reserved) (choose an appropiate region, I did Europe). Follow the instructions to create custom domains inside ngrok [here](https://ngrok.com/docs#custom-domains).

## Run git daemon

```shell
git clone https://github.com/gittogreat/git-http-server2
cd git-http-server2
npm install -g
```

## Serve repositories

**Create a directory** to serve repositories from

```shell
cd $HOME
mkdir Repos
cd Repos
```

**Create a demo repository**

```
git init --bare demo
```

**Find out the public IP** of your attendees (this assumes you are all behind the same IP)

```shell
curl ifconfig.co/ip
31.151.65.24
```

**Serve repositories**

```shell
git-http-server2 --ip 31.151.65.24 --allowcreation /Users/oliver/Repos &
listening on http://0.0.0.0:8174 in /Users/oliver/Repos
```

**Start tunnel**

```shell
ngrok start git
```

## Usage

Now you can not only serve all kinds of repositories but you can also allow your attendees to share their creations with you!

The command line flag `--allowcreation` allows for pushing repositories to your machine even if they don't exist yet! They will automatically be created!

To have Jane push her repository to your machine just tell her:

```shell
git remote add origin git@git.oschrenk.com:jane/hello.git
git push
```

Tada! You have the repository and can show the group some things



