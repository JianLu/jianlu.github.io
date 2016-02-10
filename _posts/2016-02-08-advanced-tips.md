---
layout: post
title: "Blog Setup - Advanced Tips"
date: 2016-02-08 14:26
description: Here you'll find some tricks for advanced blog features
categories: [Tech]
tags: [jekyll]
---

## Analytics

You can count visits of your site by using [Google Analytics](http://www.google.com/analytics/).
Create an account [here](https://analytics.google.com), and then add your tracking id in `_config.xml`, it should look something like `UA-********-1`.

## Tags & Categroies

Use the following post head to set categories and tags of your posts:


~~~
layout: post
title: "Post Title"
date: yyyy-mm-dd HH:MM:SS
description: Post description
categories: [cat1, cat2]
tags: [tag1, tag2, tags]
~~~

## Comments

I use [Disqus](http://disqus.com) as the comment system of the site.
You need to register on the site and change `disqus_url` to your site url.
Leaving `comment.disqus` in `_config.yaml` file empty will disable the comments

## Baidu Share

Leaving `share.baidu` empty in `_config.yaml` file will disable the share features.

## Code Syntax Highlighting using Kramdown + Rouge

In this repository, I prefer to [kramdown](http://kramdown.gettalong.org/) to parse Markdown (.md) post files.
Note that you need to use following setting to enable Kramdown to use rouge highlighting which is supported by GitHub by default.

~~~ yaml
markdown: kramdown
kramdown:
  input: GFM # Enable GitHub Flavored Markdown (fenced code blocks)
  syntax_highlighter: rouge
~~~

Here are some examples to add code block into your post (need one line space before each block):

JSON:

~~~ json
{"employees":[
    {"firstName":"John", "lastName":"Doe"},
    {"firstName":"Anna", "lastName":"Smith"},
    {"firstName":"Peter", "lastName":"Jones"}
]}
~~~

RUBY:

~~~ ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
~~~

SQL:

~~~ sql
select count(*) as cm_content_nodes
from alf_node nd, alf_qname qn, alf_namespace ns
where qn.ns_id = ns.id
  and nd.type_qname_id = qn.id
  and ns.uri = 'http://www.alfresco.org/model/content/1.0'
  and qn.local_name = 'content';
~~~

Java:

~~~ java
private String getToken(HttpClient client) throws UnsupportedEncodingException{
  Cookie[] cookies = client.getState().getCookies();
  for (Cookie cookie : cookies){
    if (cookie.getName().equals("Alfresco-CSRFToken")){
      return URLDecoder.decode(cookie.getValue(), "UTF-8");
    }
  }
  return null;
}
~~~

## Social Icons

I use [font awesome](http://fontawesome.io/icons/) icons for social icons.
For example:

- GitHub: <i class="fa fa-github"></i>
- StackOverflow: <i class="fa fa-stack-overflow">
- LinkedIn: <i class="fa fa-linkedin"></i>
- Instagram: <i class="fa fa-instagram"></i>
- RSS: <i class="fa fa-rss"></i>

You can set these icons in `_config.yml` file.
To add more icons do following steps:

 - choose an icon you want to use: [Font Awesome Icons](https://fortawesome.github.io/Font-Awesome/icons/)
 - add variable in `_config.yml`
 - add icon in `social.html` with check if variable exists:

~~~ html
{% raw %}
 {% if site.social.rss %}
  <li>
    <a title="{{ site.social.<your_social_variable> }}"
       href="{{site.url}}/{{ site.social.<your_social_variable> }}"
       target="_blank"><font_awesome_icon></i></a>
  </li>
{% endif %}
{% endraw %}
~~~
