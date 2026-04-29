---
layout: default
title: About Us
---

## Our Mission

{{ site.data.content.home.mission_statement }}

---

## Our History

{{ site.data.content.history.content }}

---

## Governing Committee

| Role | Name | Affiliation |
|------|------|-------------|
{% for member in site.data.content.committee %}| **{{ member.role }}** | {{ member.name }} | {{ member.affiliation }} |
{% endfor %}