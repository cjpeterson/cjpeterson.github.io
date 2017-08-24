---
layout: default
---

## Other Blog

This is where my miscellaneous blog posts should appear, once I write some.

<ul>
  {% for post in site.categories.other %}
    <li>
      <a href="{{site.baseurl}}{{post.url}}">{{ post.date | date_to_string }} | {{post.title}}</a>
    </li>
  {% endfor %}
</ul>