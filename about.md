---
title: About
---

<head>
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">

<style type="text/css">
  .nounderline { text-decoration:none; }
  a { text-decoration:none; }
  i { text-decoration:none; }
</style>
</head>

<body>
<center>
<a href="http://www.subsubroutine.com" style="text-decoration:none;">
  <img src="/assets/favicon.png">
</a>
<p>
{% if site.linkedin_username %}
<div>
    <a class="nounderline" href="https://au.linkedin.com/in/{{ site.linkedin_username }}" style="text-decoration:none !important;">
      <i class="fa fa-linkedin fa-3x"></i>  <!--LinkedIn -->
    </a>
</div>
{% endif %}
<p>
{% if site.twitter_username %}
<div>
    <a href="https://twitter.com/{{ site.twitter_username }}" style="text-decoration:none !important;">
      <i class="fa fa-twitter fa-3x"></i>  <!--Twitter -->
    </a>
</div>
{% endif %}
<p>
{% if site.github_username %}
    <a href="https://github.com/{{ site.github_username }}">
      <i class="fa fa-github"></i> GitHub
    </a>
{% endif %}
<p>
</div>
</center>
</body>
