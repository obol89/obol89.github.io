---
hide:
    - footer
tags:
    - Git
    - Linux
---
# Delete git branch

The basic command syntax for deleting branch is:  
`git branch (-d | -D) [-r] <branchname>...`

Note that each local branch has a corresponding upstream branch from the remote: origin

The simplest form of the command deletes a local branch:  
`git branch -d dev`

If you delete a branch that only exists locally, with unmerged changes, you’ll lose those changes. Therefore, git will refuse to delete a branch in such a situation, by default:

error: The branch `dev` is not fully merged.
If you are sure you want to delete it, run `git branch -D dev`.

So, you need to use:  
`git branch -D dev`

To remove remote branch you need to use git push command along with the `-d` flag.  

``` bash
$ git push -d origin dev

To github.com:obol89/test.git
- [deleted] dev
```
