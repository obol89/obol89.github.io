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

- `Ctrl + a` then `|` : vertical split.
- `Ctrl + a` then `S` : horizontal split.

#### Close split

- `Ctrl + a` then `X` : closes active split.
- `Ctrl + a` then `Q` : closes all splits.

### Move around splits

- `Ctrl + a` then `n` : navigate between window sessions (lowest to highest index).
- `Ctrl + a` then `Tab` : move input cursor to the next split area.

### New command

- `Ctrl + a` then `c` : create a new window within the current screen instance.

### Other

- `Ctrl + a` then `d` : detach from the session and return to the original shell. Screen continues running in the background.
- `Ctrl + a` then `x` : lock the screen session with a password.
