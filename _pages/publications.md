---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
A most updated list of publications can be found at <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

# Publications

{% for post in site.publications reversed %}
{% include archive-single.html %}
{% endfor %}

# Abstracts/Conferences

{% for post in site.abstracts reversed %}
{% include archive-single.html %}
{% endfor %}
