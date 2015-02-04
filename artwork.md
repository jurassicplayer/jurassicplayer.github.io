---
layout: default
---

<div class="posts">
  {% for post in site.categories.arts %}
    <article class="post">    
      <span class="post-date"><h4>{{ post.date | date: "%B %e, %Y" }}</h4></span>
      <h3><a href="{{ site.baseurl }}{{ post.url }}">{{ post.date | date: "%B %e, %Y" }}</a></h3>
          
      <div class="entry" style="max-height: 250px; overflow: hidden;">
        {{ post.content | truncatewords:100}}
      </div>
      
      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
    <br>
  {% endfor %}
</div>
