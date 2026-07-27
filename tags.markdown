---
layout: default
title: Tags
permalink: /tags/
hide_from_header: true
---

<div class="list-toggle" style="margin-bottom: 20px; padding-top: 20px;">
  <h2 class="post-list-heading" style="display: inline; font-weight: normal; font-size: 1.25rem;">
    <a href="{{ '/' | relative_url }}" style="color: #828282; text-decoration: none;">Posts</a>
  </h2>
  <span style="margin: 0 10px; color: #828282;">|</span>
  <h2 class="post-list-heading" style="display: inline; font-size: 1.25rem;">
    <span style="border-bottom: 2px solid #333;">Tags</span>
  </h2>
</div>

<div class="tag-cloud" style="margin-bottom: 2rem;">
  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
    {% assign t = tag | first %}
    <a href="#{{ t | slugify }}" style="display: inline-block; padding: 5px 10px; margin: 5px; background-color: #f1f1f1; border-radius: 4px; text-decoration: none; color: #333;">{{ t }} ({{ tag | last | size }})</a>
  {% endfor %}
</div>

<div class="tag-lists">
  {% for tag in sorted_tags %}
    {% assign t = tag | first %}
    {% assign posts = tag | last %}
    
    <h2 id="{{ t | slugify }}">{{ t }}</h2>
    <ul class="post-list">
      {%- assign posts_by_ref = posts | group_by: "ref_id" -%}
      {%- for group in posts_by_ref -%}
        {%- if group.name != "" and group.name != nil -%}
          {%- assign en_post = group.items | where: "lang", "en" | first -%}
          {%- assign pt_post = group.items | where: "lang", "pt" | first -%}
          {%- assign primary_post = en_post | default: pt_post | default: group.items.first -%}
          
          <li>
            {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
            <span class="post-meta">{{ primary_post.date | date: date_format }}</span>
            <h3>
              {%- if en_post -%}
                <a class="post-link" href="{{ en_post.url | relative_url }}" style="font-size: 1.25rem;">
                  {{ en_post.title | escape }} (EN)
                </a>
              {%- endif -%}
              
              {%- if pt_post -%}
                <a class="post-link" href="{{ pt_post.url | relative_url }}" style="font-size: 1.25rem;">
                  {{ pt_post.title | escape }} (PT)
                </a>
              {%- endif -%}
            </h3>
          </li>
        {%- else -%}
          {%- for post in group.items -%}
            <li>
              {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
              <span class="post-meta">{{ post.date | date: date_format }}</span>
              <h3>
                <a class="post-link" href="{{ post.url | relative_url }}" style="font-size: 1.25rem;">
                  {{ post.title | escape }}
                </a>
              </h3>
            </li>
          {%- endfor -%}
        {%- endif -%}
      {%- endfor -%}
    </ul>
  {% endfor %}
</div>
