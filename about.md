---
title: About
---

<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">

[My other blog about data science](www.subsubroutine.com)

{% if site.linkedin_username %}
    <a href="https://au.linkedin.com/in/{{ site.linkedin_username }}">
      <i class="fa fa-linkedin"></i> fa-4x <!--LinkedIn -->
    </a>
{% endif %}

{% if site.twitter_username %}
    <a href="https://twitter.com/{{ site.twitter_username }}">
      <i class="fa fa-twitter"></i> fa-4x <!--Twitter -->
    </a>
{% endif %}
{% if site.github_username %}
    <a href="https://github.com/{{ site.github_username }}">
      <i class="fa fa-github"></i> GitHub
    </a>
{% endif %}
</ul>
