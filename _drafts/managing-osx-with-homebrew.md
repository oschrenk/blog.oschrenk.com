# Managing osx with homebrew

https://github.com/Homebrew/homebrew-brewdler

Update everything

brew update
brew upgrade

See what can be cleaned up

brew cleanup -n

This command will purge any installed but no longer active version from your system.

brew cleanup


Show installed formulae that are not dependencies of another installed formula.

brew leaves


Check all installed brews for missing dependencies

brew missing

Remove dead symlinks from the Homebrew prefix. This is generally not needed, but can be useful when doing DIY installations.

brew prune





install gems as brew

https://github.com/sportngin/brew-gem
