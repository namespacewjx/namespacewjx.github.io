---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
---

# 欢迎来到吴俊贤的博客！

浏览我的[Github](http://github.com/packagewjx)


## 文章列表

[值得去的学习网站和资源](/useful_websites)

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

## 我的笔记

- [Linux学习笔记](/studying/notes/linux/index)
- [JVM学习笔记](/studying/notes/jvm)
- [设计模式笔记](/studying/notes/design-patterns)


