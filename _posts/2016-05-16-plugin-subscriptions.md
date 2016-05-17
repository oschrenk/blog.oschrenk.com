---
layout: post
title: Getting notified of all commits to your vim plugins!
---

You don’t get enough emails? Do you want to be notified of all the commits to your favourite vim plugins? If your answer to both questions is yes, follow this guide.

If you don’t manage your vim plugins with the amazing [vim-plug](https://github.com/junegunn/vim-plug) you need to change the `grep` statement accordingly.

First, create a new [Personal Access Token](https://help.github.com/articles/creating-an-access-token-for-command-line-use/) with “Access notifications” permissions.

Then exercise some command line voodoo:

```
cd $HOME
grep "Plug" .vimrc | grep -o "'.*'" | cut -d"'" -f2 | xargs -I '{}' curl -X PUT -d '{"subscribed":true}' -u <your-username>:<your-token> https://api.github.com/repos/'{}'/subscription
```

Replace `<your-username>` with your Github account name and `<your-token>` with the personal token you just created.

To delete all the subscriptions, just send a `DELETE` request:

```
grep "Plug" .vimrc | grep -o "'.*'" | cut -d"'" -f2 | xargs -I '{}' curl -X DELETE -u <your-username>:<your-token> https://api.github.com/repos/'{}'/subscription
```

Have fun!
