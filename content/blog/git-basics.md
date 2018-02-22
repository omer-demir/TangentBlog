---
title: "Git Basics"
date: 2017-12-13T12:02:15+01:00
slug: "git-basics"
tags: ["git"]
categories: ["git"]
---

# Git Basics

## Git merge branch to master

`git checkout master`
`git merge {branch_name}`

If there is any conflict fix all conflict then
`git merge --continue`

## Update fork with latest changes

 ```git
git remote add upstream https://github.com/xxxx/xxx.git
git fetch upstream
git checkout master
git merge upstream/master
git push
 ```

## Sync between branches

```git
git checkout branch_name
git merge master
git push origin branch_name
```