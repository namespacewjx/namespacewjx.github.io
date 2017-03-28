---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
---

# 欢迎来到吴俊贤的博客！

> 博客目前仍然在建设中，简陋一点咯~

[我是谁？](/resume)（先把简历放一下，有空可以写一篇没那么无聊的介绍！）

浏览我的[Github](http://github.com/namespacewjx)

[值得去的学习网站和资源](/useful_websites)


## 文章列表

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}




