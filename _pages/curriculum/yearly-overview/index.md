---
title: "Yearly Overview"
permalink: /curriculum/yearly-overview/
layout: single
author_profile: false
sidebar:
  nav: curriculum
---

{% for date in site.data.weeks %}
{% assign week_data = date[1] %}
{% assign the_date = date[0] %}

## Week {{ week_data.week }} — {{ the_date }}

{% capture table %}
| Class | Topic | Skills | Links | Homework |
| ----- | ----- | ------ | ----- | -------- |
{% for entry in week_data.classes -%}
| {{ entry.class }} | {{ entry.topic }} | {{ entry.skills }} | {% if entry.pdfs and entry.pdfs.size > 0 %}{% for pdf in entry.pdfs %}[PDF]({{ pdf }}){% unless forloop.last %}<br>{% endunless %}{% endfor %}{% else %}-{% endif %} | {{ entry.homework }} |
{% endfor %}
{% endcapture %}

{{ table | markdownify }}

---

{% endfor %}
