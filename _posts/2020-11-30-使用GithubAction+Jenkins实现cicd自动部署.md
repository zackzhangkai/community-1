---
layout: post
published: true
title: 使用GithubAction+Jenkins实现cicd自动部署
categories: [jenkins,devops]
tags: []
---
* content
{:toc}

上篇讲了如何使用github持续ci。现在实现代码ci完成后，自动触发cd，直接将代码部署到开发环境，免去开发人员手动重启服务的动作。

Github action 可以直接设置好需要登陆主机的用户名密码后，直接调用模块执行命令就可以完成相应操作。认证既可以通过用户名/密码，也可以通过Public Key。

不要看别人的博客搭建实践，直接看官方的吧，简单高效。

https://github.com/appleboy/ssh-action
