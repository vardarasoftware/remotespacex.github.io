---
layout: home
---

<div class="prose-page container max-w-screen-md py-24 md:py-28 mx-auto px-4 sm:px-6 lg:px-8">
    <h1 class="ink-heading text-4xl font-black">Blog</h1>
    <p class="ink-body text-lg mt-6">
      Find our latest articles on ruby on rails, react from our devs.
    </p>
  <ul class="mt-12">
    {%- for post in site.posts -%}
    <li class="mt-16 list-reset">
      <time class="uppercase text-xs font-bold ink-muted">{{ post.date | date: "%b %-d, %Y" }}</time>
      <h2 class="ink-heading mt-1 text-2xl tracking-tight font-extrabold sm:leading-none md:text-3xl">
        <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </h2>
      <div class="ink-body mt-6 post-content">{{ post.excerpt }}</div>
      <div class="mt-10">
        <a class="font-black uppercase text-sm tracking-wide" href="{{ post.url | relative_url }}" style="color: var(--indigo);">Read More</a>
      </div>
    </li>
    <hr class="w-full my-12" style="background: var(--line); height: 1px; border: none;">
     {%- endfor -%}
  </ul>
</div>