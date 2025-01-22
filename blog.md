---
layout: page
permalink: /blog/
title: blog
nav: true
nav_order: 1
---

<div class="post">
  <h1>Blog Posts</h1>

  <!-- Tag filter buttons -->
  <div id="tag-filters">
    <button class="tag-btn active" onclick="filterPosts('all')">All</button>
    {% assign all_tags = site.tags | sort %}
    {% for tag in all_tags %}
      <button class="tag-btn" onclick="filterPosts('{{ tag[0] }}')">{{ tag[0] }}</button>
    {% endfor %}
  </div>

  <!-- Sort options -->
  <div id="sort-options">
    <label for="sort-select">Sort by:</label>
    <select id="sort-select" onchange="sortPosts()">
      <option value="newest">Newest First</option>
      <option value="oldest">Oldest First</option>
    </select>
  </div>

  <ul id="post-list" class="post-list">
    {% assign sorted_posts = site.posts | sort: 'date' | reverse %}
    {% for post in sorted_posts %}
      <li class="post-item" data-tags="{{ post.tags | join: ' ' }}">
        <h3>
          <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h3>
        <p class="post-meta">
          {{ post.date | date: '%B %d, %Y' }}
        </p>
        <p class="post-tags">
          {% for tag in post.tags %}
            <span class="tag">{{ tag }}</span>
          {% endfor %}
        </p>
      </li>
    {% endfor %}
  </ul>
</div>

<script>
  function filterPosts(tag) {
    const posts = document.getElementsByClassName('post-item');
    const buttons = document.getElementsByClassName('tag-btn');

    for (let btn of buttons) {
      btn.classList.remove('active');
      if (btn.textContent.toLowerCase() === tag.toLowerCase()) {
        btn.classList.add('active');
      }
    }

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
      const dateA = new Date(a.querySelector('.post-meta').textContent);
      const dateB = new Date(b.querySelector('.post-meta').textContent);
      return sortOrder === 'newest' ? dateB - dateA : dateA - dateB;
    });

    posts.forEach(post => postList.appendChild(post));
  }
</script>
