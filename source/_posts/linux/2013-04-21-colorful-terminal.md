title: Colorful Terminal
date: 2013-04-21 02:30:02
tags: [linux]
permalink: colorful-terminal
---
# 漂漂終端機懶人包

![Mac iTerm2 截圖](http://i.imgur.com/Hk9J9.png?1)
<!-- more -->
### [Mac] vi ~/.bash_profile 加入以下幾行

```
# Change prompt
PS1_OLD=${PS1}
export PS1="[\[\033[0;32m\]\w\[\033[0m\]] $ "
#enables color in the terminal bash shell
export CLICOLOR=1
#sets up the color scheme for list
export LSCOLORS=ExFxCxDxBxegedabagacad
#enables color for iTerm
#export TERM=xterm-color
export TERM=xterm-256color
#sets up proper alias c

```

### [Ubuntu] gedit ~/.bashrc 加入以下幾行

```
force_color_prompt=yes #打開註解

...
if [ "$color_prompt" = yes ]; then
    #PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    PS1="[\[\033[0;32m\]\w\[\033[0m\]] $ "
else
    #PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    PS1="[\[\033[0;32m\]\w\[\033[0m\]] $ "
fi

...

```

別把裡面其他東西給刪囉

### Reference
1. http://blog.taylormcgann.com/2012/06/13/customize-your-shell-command-prompt/
2. http://www.wretch.cc/blog/thewindjuei/21313506