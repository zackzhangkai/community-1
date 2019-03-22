---
layout: post
published: true
title:  git rebase与git merge
categories: [document]
tags: [git]
---
* content
{:toc}

### 前言

git rebase是“变基”，git log --graph时，历史提交基线变直，变得好看。

git merge是合并分支、这时一般要解决冲突，如果在有冲突时，不想解决冲突，可以用 ```git merge --abort来退回```；当然后git 任何时候都可以用git reset --hard来恢复到之前的情况

git log --pretty=oneline 一行显示每次的历史提交。
