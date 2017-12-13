---
title: "How to Use Hugo"
date: 2017-12-13T12:02:15+01:00
slug: "how-to-use-hugo"
tags: ["hugo"]
categories: ["hugo"]
---

# Hugo a brand new way for static site

Hugo is a static website generation with GoLang power. It is easy to use and quick to put on production.

First thing first;

## Installing hugo to windows/linux based OS

- For windows we can use simple command `choco install hugo -confirm`
- For other OS's we can use brew like `brew install hugo`

## Creating website with Hugo CLI

All you need to do is simple run the command `hugo new site quickstart`

## We can add theme for our website

First we need to find theme, then we need to add it as submodule and define in config file of Hugo.

- In order to find theme go to this web site [Hugo themes](https://themes.gohugo.io/).
- Add as a submodule and initialized in **theme** fodler `git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke;`
- Add theme definition in **config.toml** `theme = "ananke"`

Voila!! you have theme enabled hugo.

## Lets add some content

Hugo is serving your content with Markdown files. you can have different type of files as well. As an example run `hugo new posts/my-first-post.md`

## Lets run hugo

All you need to do is `hugo server -D`