---
layout: page
title: Blog
permalink: /blog/
---

<div class="wrapper post">
  <header class="header">
    <h1 class="header-title center">{{ page.title }}</h1>
  </header>

  {% for post in site.posts %}
    <article class="post-item">
      <h2 class="post-item-title">
        <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </h2>
      <div class="post-meta">
        <time datetime="{{ post.date | date_to_xmlschema }}">
          {{ post.date | date: "%b %-d, %Y" }}
        </time>
        {% if post.tags.size > 0 %}
          <span class="tags">
            {% for tag in post.tags %}
              <a class="tag" href="{{ site.baseurl }}/tags/#{{tag}}">{{ tag }}</a>
            {% endfor %}
          </span>
        {% endif %}
      </div>
      {% if post.description %}
        <p class="post-item-description">{{ post.description }}</p>
      {% endif %}
    </article>
  {% endfor %}
</div>
