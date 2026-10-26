---
layout: editorial
title: Writing
description: "Perspectives on software craft, engineering culture, and building with an AI harness — from the engineers at Vardara Software."
permalink: /blog/
redirect_from:
  - /blog
---

<!-- BLOG HERO -->
<section class="bl-hero" aria-labelledby="bl-heading">
  <div class="ed-wrap">
    <p class="bl-kicker">Writing</p>
    <h1 id="bl-heading" class="bl-h1">Perspectives on<br><em>software craft.</em></h1>
    <p class="bl-lead">Engineering culture, system design, and building software with an AI harness — from the lab.</p>
  </div>
</section>

<!-- POST LIST -->
<section aria-label="Articles">
  <div class="ed-wrap">
    <ul class="bl-list" role="list">
      {%- for post in site.posts -%}
      <li class="bl-item">
        <div class="bl-item-meta">
          <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
          {%- if post.tags.size > 0 -%}
          <span aria-hidden="true">/</span>
          <div class="bl-item-tags" aria-label="Tags">
            {%- for tag in post.tags -%}
            <span class="bl-item-tag">{{ tag | escape }}</span>
            {%- endfor -%}
          </div>
          {%- endif -%}
        </div>
        <h2 class="bl-item-title">
          <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        </h2>
        {%- if post.description -%}
        <p class="bl-item-excerpt">{{ post.description | escape }}</p>
        {%- elsif post.excerpt -%}
        <p class="bl-item-excerpt">{{ post.excerpt | strip_html | truncate: 200 }}</p>
        {%- endif -%}
        <a class="bl-read-more" href="{{ post.url | relative_url }}" aria-label="Read {{ post.title | escape }}">
          Read
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
      </li>
      {%- endfor -%}
    </ul>
  </div>
</section>
