---
layout: page
title: Talks
permalink: /talks/
---

Presenting is incredible opportunity to give back to the community and help both yourself and others learn.  I've been incredibly lucky to have had tremendous opportunity to present throughout my career.  

Presenting also affords an amazing platform for refining your knowledge on a topic or exploring and engaging in different ideas.  I'm very fortunate to be able to present to many different groups on many different ideas.  

These are some of my more recent talks.

<ul class="post-list">
  {% for post in site.categories.talks %}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

      <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>
    </li>
  {% endfor %}
</ul>
