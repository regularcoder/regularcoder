---
layout: page
title: Archive
---

<table class="table">
{% for post in site.posts %}
    {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
    {% if year != y %}
      {% assign year = y %}
      {% raw %}
      <tr><td colspan="2"><h3>
      {% endraw %}{{ y }}</h3></td></tr>
    {% endif %}
    <tr>
      <td class="text-nowrap"><time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%B %d, %Y" }}</time></td>
      <td><a href="{{ post.url | prepend: site.url }}" title="{{ post.title }}">{{ post.title }}</a></td>
    </tr>
{% endfor %}
</table>