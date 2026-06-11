---
layout: home
---

# You can reach me via afterjourney1993@gmail.com
# MY BLOG LIST

{% for post in site.posts %}
## [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
📅 {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
