---
layout: post
title: "Blog Setup"
date: 2016-02-07 12:13
categories: [Tech]
tags: [jekyll]
description: How to use this Jekyll blog template
---

## Blog repository

To create your own blog from scratch, you can clone or fork this repository  from
[my GitHub blog repository](https://github.com/JianLu/jianlu.github.io).

If you want to edit and review your blog locally, you need to install ruby and jekyll,
the [Jekyll Documentation] shows you how to install all requirements nad how to configure.

## Customize your site

### Configuration file

You can set up your own information in `_config.yml` file. Set the information in the section of `Site settings`.
In the `Build settings` section, I included some features like pagination, code syntax highlighting.

### Add your blog
You can put your articles in the `_post` folder, refering this file as the template.

### Review your site
If you have installed **Jekyll** and the dependencies, you can host your site locally on your machine.
Go to the site folder, and type

~~~ bash
jekyll serve
~~~

Then you can open `127.0.0.1:4000` in the browser to review your site.
