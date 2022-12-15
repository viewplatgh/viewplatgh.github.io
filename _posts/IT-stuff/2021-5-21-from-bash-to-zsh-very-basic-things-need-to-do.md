---
layout: post
title: From bash to zsh, very basic things need to do
tag: it-stuff
---

After upgrading MacOS to Catalina, everytime opening a terminal, there would be a prompt saying:

```console
The default interactive shell is now zsh.
To update your account to use zsh, please run `chsh -s /bin/zsh`
For more details, please visit https://support.apple.com/kb/HT208050
...
```

One day, I couldn't stand it anymore, I just run the command `chsh -s /bin/zsh` and then password.
Then I run terminal.app, the annoying prompt disappeared forever.
However, the new problem is the alias I made in .bashrc or .profile wouldn't work anymore...
Here is the solution to the problem:

1. Have a file ~/.zprofile, and have one line in it

```console
[[ -e ~/.profile ]] && emulate sh -c 'source ~/.profile'
```

2. Have a file ~/.zshrc, and have one line in it

```console
[[ -e ~/.bashrc ]] && emulate sh -c 'source ~/.bashrc'
```
