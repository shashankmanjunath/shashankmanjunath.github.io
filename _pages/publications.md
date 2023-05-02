---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if site.author.googlescholar %}
A most updated list of publications can be found at <a href="{{ site.author.googlescholar }}">my Google Scholar profile</a>.
{% endif %}

{% include base_path %}

## Peer-Reviewed Publications

<ol>
  {% for post in site.publications reversed %}
    {% include archive-single-short.html %}
  {% endfor %}
</ol>

## Preprints

<ol>
  {% for post in site.preprints reversed %}
    {% include archive-single-short.html %}
  {% endfor %}
</ol>

## Abstracts/Conferences

<ol>
  {% for post in site.abstracts reversed %}
    {% include archive-single-short.html %}
  {% endfor %}
</ol>

## Master's Thesis

Shashank Manjunath. <a href="https://open.bu.edu/handle/2144/44776">Machine
Learning Techniques for Reconstruction and Segmentation of Nanoparticle
Interferometric Signatures,</a> Master's Thesis, Boston University. May 2022.
