---
excerpt: ""
comments: true
title: Support $\LaTeX$ on websites
categories:
  - blog build
---

## MathJax

[MathJax](https://www.mathjax.org/) is an elegent way to support $\LaTeX$ on websites. The syntax is similar with that in Latex such as `$3^2 + 4^2 = 5^2$` and `$$e^{i\pi} + 1 = 0$$`, which are shown as $3^2 + 4^2 = 5^2$ and 

$$
e^{i\pi} + 1 = 0.
$$

## Configuration

To use MaxJax in your web pages, you only need to put several lines of codes in the `<head>` block of your html document. The following are the ones I am using. Basically, `tex2jax` tells the engine what are the delimiters for formulas. By default, MaxJax doesn't support \$\$ as there are so many such symbols in websites. So after turning this option on, you need to use `processEscapes: true` to tell the engine that `\$` represents one single dollar symbol. `TeX` tells the engine how to render formulas where you can specify macros.

```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'] ],
      processEscapes: true
    },
    TeX: { 
      equationNumbers: { 
        autoNumber: "AMS" 
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

Another feature I like is MaxJax by default also supports cross references outside formula delimiters.

If you are running a blog, a convenient way is to put this script in the header file that are included by every html file.

For more details of the configuration, you can check the [documentation](http://docs.mathjax.org/en/latest/tex.html).

