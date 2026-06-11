---
layout: home
---

# MY BLOG LIST
## You can reach me via afterjourney1993@gmail.com

{% for post in site.posts %}
## [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
📅 {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
