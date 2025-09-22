---
title: git：如何更新main分支
date: 2025-09-22 18:31:19
tags:
  - git
categories:
---

## fetch - Download objects and refs from another repository

```bash
git fetch --prune --all
```

```bash
git switch main
git fetch [origin] [main]
```

```bash
git fetch origin main:main
```

## pull - Fetch from and integrate with another repository or a local branch

```bash
git switch main
git pull [origin] [main]
git pull --rebase
```

## merge - Join two or more development histories together

```bash
git switch main
git merge origin/main
```

## rebase - Reapply commits on top of another base tip

```bash
git switch main
git rebase origin/main
```

```bash
git rebase origin/main main
```

```bash
git rebase --onto <newbase> <upstream> <branch>
# <newbase> default: <upstream>
# <branch> default: HEAD
```

## branch - List, create, or delete branches

```bash
git branch main
```

## switch - Switch branches

```bash
git switch -c main
```

## restore - Restore working tree files

## checkout - Switch branches or restore working tree files

```bash
git checkout main
```

## reset - Reset current HEAD to the specified state

```bash
git switch main
git reset origin/main
```
