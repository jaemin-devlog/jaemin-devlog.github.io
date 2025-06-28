---
layout: page
title: 홈
permalink: /
---

환영합니다! 이 블로그는 개발과 관련된 내용을 공유하는 공간입니다.  
👉 좌측 메뉴에서 카테고리별 포스트를 확인해보세요.

---

## 📝 최근 게시물

<div class="post__list">
  {% for post in site.posts limit:5 %}
    <article class="post-preview">
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt | strip_html | truncatewords: 20 }}</p>
      <small>{{ post.date | date: "%Y-%m-%d" }}</small>
    </article>
    <hr />
  {% endfor %}
</div>
