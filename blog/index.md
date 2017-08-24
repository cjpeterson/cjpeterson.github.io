---
layout: default
---

## Blog

This is where the blog posts should appear, once I figure that out.

<ul>
  {% for post in site.categories.blog %}
    <li>
      <a href="{{site.baseurl}}{{post.url}}">{{ post.date | date_to_string }} | {{post.title}}</a>
    </li>
  {% endfor %}
</ul>
