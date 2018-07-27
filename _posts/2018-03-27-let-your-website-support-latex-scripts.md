---
excerpt: ""
comments: true
title: Support Latex scripts on websites
categories:
  - blog build
---

## MathJax

[MathJax](https://www.mathjax.org/) is an elegent way to support mathmatical equations on websites. The syntax is similar with that in Latex such as `$3^2 + 4^2 = 5^2$` and  `$$e^{i\pi} + 1 = 0$$`, which are shown as $3^2 + 4^2 = 5^2$ and 

$$e^{i\pi} + 1 = 0.$$

To use it, just put the following code inside every header of html files.

```
<script type="text/x-mathjax-config">
 MathJax.Hub.Config({
   tex2jax: {
     inlineMath: [ ['$','$'], ['\\(','\\)'] ],
     processEscapes: true
   }
 });
</script>
<script type="text/x-mathjax-config"> 
  MathJax.Hub.Config({ 
    TeX: { 
      equationNumbers: { 
        autoNumber: "all" 
      },
      Macros: {
        eps: "{\\epsilon}",
        E: "{\\mathrm{E}}",
        Pr: "{\\mathrm{Pr}}"
      } 
    } 
  }); 
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-AMS_CHTML">
</script>
```

If you are running a blog, the correct way is to put these scripts in the header file that are included by every html file.

For more details of the configuration, you can check the [documentation](http://docs.mathjax.org/en/latest/tex.html).

## Notes

Note that `processEscapes: true` is necessary if you want to input a single dollar symbol using `\\$`.

