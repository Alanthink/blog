---
excerpt: ""
comments: true
title: Let your website support Latex scripts
categories:
  - blog build
---

## MathJax

[MathJax](https://www.mathjax.org/) is an elegent way to support mathmatical equations on websites. The syntax is similar with that in Latex such as `$ 3^2 + 4^2 = 5^2 $` and  `$$ e^{i\pi} + 1 = 0 $$`, which are shown as

$3^2 + 4^2 = 5^2$ and 

$$e^{i\pi} + 1 = 0.$$


To use it, just put the following code inside every header of html files.

```
<script type="text/x-mathjax-config"> 
MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } }); 
</script>
<script type="text/x-mathjax-config">
 MathJax.Hub.Config({
   tex2jax: {
     inlineMath: [ ['$','$'], ["\\(","\\)"] ],
     processEscapes: true
   }
 });
</script>
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript">
</script>
```

For more details, please check the [documentation](http://docs.mathjax.org/en/latest/start.html).

## Notes

Note that `processEscapes: true` is essential otherwise `$$` will not work as what we were imaging.

