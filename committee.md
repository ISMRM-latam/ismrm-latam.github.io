---
layout: default
title: Governing Committee
---
| Role | Name | Affiliation |
|------|------|-------------|
{% for member in site.data.content.committee %}| **{{ member.role }}** | {{ member.name }} | {{ member.affiliation }} |
{% endfor %}