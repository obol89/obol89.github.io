---
comments: true
hide:
    - footer
tags:
    - Git
    - Linux
---
# Delete git branch

Syntax:

```text
git branch (-d | -D) [-r] <branchname>...
```

Delete a local branch:

```bash
git branch -d dev
```

If the branch has unmerged changes, git will refuse:

```text
error: The branch `dev` is not fully merged.
If you are sure you want to delete it, run `git branch -D dev`.
```

Force-delete a local branch with unmerged changes:

```bash
git branch -D dev
```

Delete a remote branch:

```bash
git push -d origin dev
```

```text
To github.com:obol89/test.git
- [deleted] dev
```
