---
layout: page
permalink: /blog/
title: blog
nav: true
nav_order: 1
---

<div class="post">

  {% if site.blog_name or site.blog_description %}
    <div class="header-bar">
      <h1>{{ site.blog_name }}</h1>
      <h2>{{ site.blog_description }}</h2>
    </div>
  {% endif %}

  <div class="tag-category-list">
    <ul class="p-0 m-0">
      <li>
        <a href="#" onclick="filterPosts('all'); return false;">
          <i class="fa-solid fa-asterisk fa-sm"></i> All
        </a>
      </li>
      {% assign all_tags = site.tags | sort %}
      {% for tag in all_tags %}
        <li>
          <a href="#" onclick="filterPosts('{{ tag[0] }}'); return false;">
            <i class="fa-solid fa-hashtag fa-sm"></i> {{ tag[0] }}
          </a>
        </li>
      {% endfor %}
    </ul>
  </div>

  <div class="post-list-header">
    <h2>Posts</h2>
    <div class="sort-options">
      <label for="sort-select">Sort by:</label>
      <select id="sort-select" onchange="sortPosts()">
        <option value="newest">Newest First</option>
        <option value="oldest">Oldest First</option>
      </select>
    </div>
  </div>

  <ul id="post-list" class="post-list">
    {% assign sorted_posts = site.posts | sort: 'date' | reverse %}
    {% for post in sorted_posts %}
      <li class="post-item" data-tags="{{ post.tags | join: ' ' }}">
        <h3>
          <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h3>
        <p>{{ post.description }}</p>
        <p class="post-meta">
          {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
          {{ read_time }} min read &nbsp; &middot; &nbsp;
          {{ post.date | date: '%B %d, %Y' }}
        </p>
        <p class="post-tags">
          <a href="{{ post.date | date: '%Y' | prepend: '/blog/' | prepend: site.baseurl}}">
            <i class="fa-solid fa-calendar fa-sm"></i> {{ post.date | date: '%Y' }}
          </a>
          {% if post.tags.size > 0 %}
            &nbsp; &middot; &nbsp;
            {% for tag in post.tags %}
              <a href="{{ tag | slugify | prepend: '/blog/tag/' | prepend: site.baseurl}}">
                <i class="fa-solid fa-hashtag fa-sm"></i> {{ tag }}
              </a>
              {% unless forloop.last %}&nbsp;{% endunless %}
            {% endfor %}
          {% endif %}
        </p>
      </li>
    {% endfor %}
  </ul>
</div>

<script>
  function filterPosts(tag) {
    const posts = document.getElementsByClassName('post-item');
    for (let post of posts) {
      if (tag === 'all' || post.dataset.tags.includes(tag)) {
        post.style.display = '';
      } else {
        post.style.display = 'none';
      }
    }
  }

  function sortPosts() {
    const postList = document.getElementById('post-list');
    const posts = Array.from(postList.getElementsByClassName('post-item'));
    const sortOrder = document.getElementById('sort-select').value;

    posts.sort((a, b) => {
      const dateA = new Date(a.querySelector('.post-meta').textContent.split('·')[1].trim());
      const dateB = new Date(b.querySelector('.post-meta').textContent.split('·')[1].trim());
      return sortOrder === 'newest' ? dateB - dateA : dateA - dateB;
    });

    posts.forEach(post => postList.appendChild(post));
  }
</script>
