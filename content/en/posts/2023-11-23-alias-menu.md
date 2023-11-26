---
title: "Automatic help for your shell aliases"
description: ""
date: 2023-11-23T18:03:42+01:00
featured: false
draft: false
comment: true
toc: false
reward: true
categories:
tags:
images:
  - /images/feature/shell.png
---

A few days ago I have discovered [dotree](https://github.com/KnorrFG/dotree), a nice litte tool that "wants to be a 
better home for your aliases and bash functions".

This made me think: do I really need another tool or can I obtain a similar result with a zsh function?

So armed with impatience I have asked ChatGPT to write the `al` function. After a bit of tweaking the resulting code is 
fairly decent and does the job. You can find it in this 
[gist](https://gist.github.com/simgunz/893cfe99971e2813e01655db82cc837d).

---

I have my aliases defined in `~/.zsh/zalias` like this:

```zsh
## Misc
alias llg='ls -l | grep'      # List files and grep
alias lag='ls -la | grep'     # List hidden files and grep

## Manjaro
alias pci='sudo pacman -Sy'   # Install a package
alias pcr='sudo pacman -R'    # Remove a package
alias pcu='yay -Syu'          # Update packages
```

Executing `al` I am presented with the following menu:

```zsh
Select a category:
1) Misc
2) Manjaro
?#
```

Selecting category `2) Manjaro`:

```zsh
Select an alias from 'Manjaro':
2) pci: install a package
3) pcr: remove a package
4) pcu: update packages
?#
```

and finally selecting `4) pcu` the alias command is displayed in case we want to inspect it

```zsh
yay -Syu
```

Differently from dotree, this function does not execute the selected command because most of my aliases
require additional arguments so it would not make sense to executed them from the menu.