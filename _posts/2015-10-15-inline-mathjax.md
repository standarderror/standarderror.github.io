---
title: inline MathJax on Github Pages
updated: 2015-10-20
---


## The scripts I had to add to my github pages site to get inline MathJax:

```HTML
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script>
MathJax.Hub.Config({
  tex2jax: { inlineMath: [['$', '$'], ['\\(', '\\)']] }
});
</script>
```

Now I can write `$\frac{1}{x^2}$` and get $\frac{1}{x^2}$

[Thank you](http://jwinder.io/blogging-with-jekyll-github-pages-and-supportkit/)
