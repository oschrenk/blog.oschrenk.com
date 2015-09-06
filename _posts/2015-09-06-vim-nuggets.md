---
layout: post
title: Vim nuggets
---

Looking at the [history of my .vimrc](https://github.com/oschrenk/dotfiles/commits/master/.vimrc) on Github, it seems that I started using vim professionally a bit over one year ago. I remember really humble beginnings, reading a lot of tutorials, watching a lot of videos (which just now actually all start to make sense), writing my own cheatsheets, playing the amazing (but arguably overpriced) [vim adventures](http://vim-adventures.com/) and still feeling overwhelmed with vim veterans sitting next to you badmouthing your typing skills while the brain tries to rewire itself and the fingers try to unknot themselves.

But times have changed and now I really comfortable and really enjoying vim. I embraced the diea of modal editing and just love that the editor feels like a physical tool growing with your skills and that the `.vimrc` is a representation of that.

Looking back now these are some of the nuggets of `.vimrc` snippets I enjoy.

## Disable backups

I use Dropbox and git and honestly don't remember the last time I had to recover some files from a backup. With modern hardware and tooling, I think there is no need for them.

```vim
set nobackup                  " Get rid of backups, I don't use them
set nowb                      " Get rid of backups on write
set noswapfile                " Get rid of swp files, I have never used them
```

## Persistent undo

The one thing I do love though, is having a local history of my changes on top of git. It is so super handy when you move around different file during the day, but still be able to undo the last step.

```vim
" Keep undo history across sessions
" see :help undo-persistence
if exists("+undofile")
  " create dir if it doesn't exist
  if isdirectory($HOME . '/.vim/undo') == 0
    :silent !mkdir -p ~/.vim/undo > /dev/null 2>&1
  endif
  " if path ends in two slashes, file name will use complete path
  " :help dir
  set undodir=~/.vim/undo//
  set undofile
  set undolevels=500
  set undoreload=500
endif
```

## When committing, start in insert mode

This is such an amazing little time saver when doing quick commits. Once you set this one up you will ask yourself how you were even able to use vim as your `$EDITOR` before.

```vim
" when commiting  add new line and enter insert mode
au FileType gitcommit startinsert
```

## Jump to the last cursor position when opening a file

This will save you a lot of mental overhead, looking for your latest changes. I'm not sure where exactly I found it but I traced it back to [srooloose vimrc](https://github.com/scrooloose/vimfiles/blob/f6290aafdf81685dff29e5b185971a809a08ad7d/vimrc#L283)

```vim
"jump to last cursor position when opening a file
"dont do it when writing a commit log entry
autocmd BufReadPost * call SetCursorPosition()
function! SetCursorPosition()
    if &filetype !~ 'svn\|commit\c'
        if line("'\"") > 0 && line("'\"") <= line("$")
            exe "normal! g`\""
            normal! zz
        endif
    end
endfunction
```

## Moving to the beginning of the line

For quite a while I had these settings

```vim
noremap H 0     " Jump to beginning of line
noremap L $     " Jump to end of line
```

But what I normally wanted to do was jump to the first character of the line and not the beginning of the line itself, just because its more probable that I am moving through code and the current line has some level of indentation.

Turns out [somebody else](http://jezenthomas.com/moving-to-the-beginning-of-the-line/) had the same issue and solved it:

```vim
" Jump to first character or column
noremap <silent> H :call FirstCharOrFirstCol()<cr>

function! FirstCharOrFirstCol()
  let current_col = virtcol('.')
  normal ^
  let first_char = virtcol('.')
  if current_col == first_char
    normal 0
  endif
endfunction
```

