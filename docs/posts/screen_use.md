---
comments: true
hide:
    - footer
tags:
    - Linux
    - Tools
---
# How to use basic screen commands on Linux

List **screen** sessions:

```bash
screen -ls
```

Name and open new **screen** session:

```bash
screen -S session_name
```

Reattach screen session:

```bash
screen -r sessions_name
```

## Screen shortcuts

### Split screen

#### New split

- `Ctrl + a` then `|` : vertical split. By default, this command will split your current window into two different areas that you can interact with.
- `Ctrl + a` then `S` : horizontal split.

#### Close split

- `Ctrl + a` then `X` : closes active split.
- `Ctrl + a` then `Q` : closes all splits.

### Move around splits

- `Ctrl + a` then `n` : used to navigate between your different window sessions from the one with the lowest index until the one with the greater index.
- `Ctrl + a` then `Tab` : used in order to move your input cursor to one of your different areas within screen.

### New command

- `Ctrl + a` then `c` : screen command. One of the most used commands by far, those bindings are used in order to create a new window within your screen instance.

### Other

- `Ctrl + a` then `d` : detach mode. You can use this option in order to detach (meaning going back to your original shell) and let screen run in the background. This is particularly handy when you are running long tasks on your host.
- `Ctrl + a` then `x` : lockscreen. This is used in order to protect your screen session to be used by another user. As a consequence, a password is set to your session.
