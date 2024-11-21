---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if site.author.googlescholar %}
  <div class="wordwrap">A most updated list of publications can be found at <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
{% endif %}

{% include base_path %}

## Journal Articles

<ol>
  {% for post in site.publications reversed %}
    {% if post.category == 'manuscripts' %}
      {% include archive-single-short.html %}
    {% endif %}
  {% endfor %}
</ol>

## Conference Papers

<ol>
  {% for post in site.publications reversed %}

    {% if post.category == 'conferences' %}
      {% include archive-single-short.html %}
    {% endif %}

  {% endfor %}
</ol>

## Preprints

<ol>
  {% for post in site.publications reversed %}
    {% if post.category == 'preprints' %}
      {% include archive-single-short.html %}
    {% endif %}
  {% endfor %}
</ol>

## Master's Thesis
<ol>
  {% for post in site.publications reversed %}
    {% if post.category == 'masters-thesis' %}
      {% include archive-single-short.html %}
    {% endif %}
  {% endfor %}
</ol>
