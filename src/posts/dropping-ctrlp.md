---
title: "Dropping CtrlP and other plugins"
date: "2015-08-01"
tags: ["buffer", "tag", "find", "grep", "plugin"]
collection: "posts"
template: "post.html"
---
At the end of July I dropped [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) and
other plugins from my [Vim](http://www.vim.org/) setup. CtrlP was one of the
first that I installed and until a couple of weeks ago I would've deemed it an
essential plugin.

The truth is that I never learned how to navigate effectively with Vim's
built-in commands. I know how to use `:e` and its companions, but beyond that I
never gave it any thought.

It turns out that Vim's tab completion, coupled with the `wildmenu` option,
works really well since it can complete everything from buffers, unopened files
to tags.

## Built-ins

### Navigating buffers

I use this for listing and switching buffers:

``` vim
nnoremap § :ls<cr>:b
```

The `§` key is on the same row as the numbers on my keyboard, which makes it
fitting. I also have this in my `.vimrc`:

``` vim
nnoremap <BS> <C-^>
```

This will jump to the last edited file. Typing `^` requires two keystrokes for
me, hence the remapping.

### Navigating tags

``` text
:tag <name>
```

You can also search for a tag by pattern:

``` text
:tag /<pattern>
```

I find tags extra useful when I'm dealing with CSS. If I want to jump to a class
named `.block` I can do `:tag .bl`, press `<Tab>` and then hit `<Enter>`.

### Finding files

``` text
:find <filename>
```

I unfortunately find tab completion to be a bit slow with the above command, but
since I don't work with large codebases I don't use it that much.

To make it work recursively you need to modify the `path` option:

``` vim
set path+=**
```

You also need to make sure to modify the `wildignore` option too, unless you
want to include files that belong to your dependencies:

``` vim
set wildignore+=*/node_modules/*,*/vendor/*
```

### Grepping

I regularly grep for all kinds of things. I've used
[ack.vim](https://github.com/mileszs/ack.vim),
[ctrlsf.vim](https://github.com/dyng/ctrlsf.vim) and I even wrote my own
[plugin](https://github.com/gummesson/vim-grepany). I ended up removing it and
changing the `grepprg` and `grepformat` options instead:

``` vim
if executable('ack')
  set grepprg=ack\ -s\ -H\ --nogroup\ --nocolor\ --column
  set grepformat=%f:%l:%c:%m,%f:%l:%m
endif
```

I use [ack](http://beyondgrep.com/) since it works well on both Linux and
Windows systems. I also added a small helper command, courtesy of
[thoughtbot](https://thoughtbot.com/)'s guide on "[Faster Grepping in
Vim](https://robots.thoughtbot.com/faster-grepping-in-vim)":

``` vim
command! -bang -nargs=* -complete=file -bar Grep silent! grep! <args>
```

Which I then use like this:

``` text
:Grep <args>
```

If I want to use [git-grep](http://git-scm.com/docs/git-grep) instead of ack I
run `:Ggrep`, a feature from
[vim-fugitive](https://github.com/tpope/vim-fugitive).

One of the nicest features that comes with ack.vim and other grepping plugins is
that the quickfix window opens automatically after searching. Luckily, it's not
hard to enable that either:

``` vim
autocmd QuickFixCmdPost *grep* cwindow
```

The above snippet comes from [vim-fugitive's
FAQ](https://github.com/tpope/vim-fugitive#faq).

## Alternatives

When I removed CtrlP I actually swapped it for another plugin,
[DidYouMean](https://github.com/EinfachToll/DidYouMean). I think the author
describes its use case best:

> If you're like me and you want to edit a specific file with Vim, say,
> `test.py`, you type `vim te` into the terminal, then you hit `<Tab>` and
> `<Enter>` immediately because you think your shell expands the characters to
> the right file name. But if there's another file starting with `te`, Vim fires
> up with an empty file called `te`, laughing at you. That's annoying. This
> simple plugin makes Vim ask for the right file to open.

The plugin is only a handful of lines long, but it saves me from much so 
frustration.
