---
layout: post
title: Writing Command Line Tools in Ruby
tags:
- ruby
- cli
---

At my current [day job](http://www.vakantiediscounter.nl/) we use several programming languages, and I like to think that we choose the language depending on the problem we are trying to solve. In the  backend we chose Scala as our main language because we believe that the data processing tools we need, do existin the JVM space and are of high quality. In the frontend we chose to go with ClojureScript because we believe in the functional programming paradigm, because we believe in JavaScript only as compilation target and because we believe that you can be far more productive.

That leaves with some maintenance and scripting needs. We use Ruby.

## I need

**Nested subcommands**

```
mario solr restart --env=prod
mario spark
mario tui process increments --env=prod --force

```

## I want

**Auto completion**

It would be so great if there would be a framework that would take care of creating the various auto-completion scripts for you. Why do we need to write this separately? If I add a new command, or a new parameter, I want to generate new `bash`, `zsh`, and `fish` auto completion scripts to be generated for me.

**Negative flags**

The command line tool should auto-generate command line options for negating a boolean flag. We are often defaulting to a certain set of input sources, but need to be able to quickly manipulate these inputs source by white-listing certain ones, or by using the default set but black-listing one ore more specific sources.

For example:


## Solution space

There are two approaches

1. Using a framework
2. Using a library

### Frameworks

These are full solutions, offering one or even more parsing solutions, creating templates, and some even auto-generate documentation and tests for you.

**[main](https://github.com/ahoward/main)**

>  a class factory and dsl for generating command line programs real quick

**[cri](https://github.com/ddfreyne/cri)**

> A tool for building commandline applications

- [commander](https://github.com/commander-rb/commander) Looks promising, but is very thin on documentation of sub-commands
- [slop](https://github.com/leejarvis/slop) no sub-commands
- [Thor](https://github.com/erikhuda/thor)
    + no option validation
- [optitron](https://github.com/joshbuddy/optitron) no subcommands

### Libraries

- [choice](https://github.com/defunkt/choice) no subcommands

**[highline](https://github.com/JEG2/highline)**

> A higher level command-line oriented interface

A library that eases the task of dealing with input and output on the terminal. It requests data from the user via the `ask` and waits on input by the user (with hidden input support for passwords and sensitive data). Furthermore it can help you build menus.

### The fringe

These are applications/frameworks/libraries which might be useful for ceratin aspects of writing command line applications.

- [aruba](https://github.com/cucumber/aruba) testing of commandline applications for cucumber, including file system operations
- [fakefs](https://github.com/defunkt/fakefs) Fakes filesystem for testing (for mocha)
- [net-ssh](https://github.com/net-ssh/net-ssh) Pure Ruby implementation of  SSH protocol
- [paint](https://github.com/janlelis/paint) Manipulate terminal colors
- [ronn](https://github.com/rtomayko/ronn) Build manuals
- [sysexits](https://github.com/ged/sysexits/) Sysexit definitions

## Resources

- [Documenting Ruby command-line apps](http://blog.excelwithcode.com/documenting-commandline-apps.html)
