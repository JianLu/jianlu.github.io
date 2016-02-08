---
layout: post
title: "Blog Setup"
date: 2016-02-07 12:13
categories: [Tech]
tags: [jekyll]
description: Here you'll find out how to install this theme
---

# Blog repository

To create your own blog from scratch, you can clone or fork this repository  from
[my GitHub blog repository](https://github.com/JianLu/jianlu.github.io).

If you want to edit and review your blog locally, you need to install ruby and jekyll,
the [Jekyll Documentation] shows you how to install all requirements nad how to configure.

# Customize your site

## Configuration file

You can set up your own information in `_config.yml` file. Set the information in the section of `Site settings`.
In the `Build settings` section, I included some features like pagination, code syntax highlighting.

## Add your blog
You can put your articles in the `_post` folder, refering this file as the template.

## Review your site
If you have installed **Jekyll** and the dependencies, you can host your site locally on your machine.
Go to the site folder, and type

~~~ shell
jekyll serve
~~~

Then you can open `127.0.0.1:4000` in the browser to review your site.

# Tech Tips

## Code Syntax Highlighting using Kramdown + Rouge

In this repository, I prefer to [kramdown](http://kramdown.gettalong.org/) to parse Markdown (.md) post files.
Note that you need to use following setting to enable Kramdown to use rouge highlighting which is supported by GitHub by default.

~~~ yaml
markdown: kramdown
kramdown:
  input: GFM # Enable GitHub Flavored Markdown (fenced code blocks)
  syntax_highlighter: rouge
~~~

