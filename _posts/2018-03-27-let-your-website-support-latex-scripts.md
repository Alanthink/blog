---
excerpt: ""
comments: true
title: Support $\LaTeX$ on websites
categories:
  - cs
tags:
  - blog build
---

## MathJax

[MathJax](https://www.mathjax.org/) is an elegent way to support $\LaTeX$ on websites. The syntax is similar with that in Latex such as `$3^2 + 4^2 = 5^2$` and `$$e^{i\pi} + 1 = 0$$`, which are shown as $3^2 + 4^2 = 5^2$ and 

$$
e^{i\pi} + 1 = 0.
$$

## Configuration

To use MaxJax in your web pages, you only need to put several lines of codes in the `<head></head>` block of your html document. The following is the one I am using. Basically, `tex2jax` tells the engine what are the delimiters for formulas. By default, MaxJax doesn't support `$$` inline equations as it usually represents US dollar. So after turning this option on, you need to use `processEscapes: true` to tell the engine that only `\$` represents one single dollar symbol. `TeX` tells the engine how to render formulas where you can specify macros.

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
        E: "{\\mathbb{E}}",
        Pr: "{\\mathrm{Pr}}",
        cond: "{~|~}",
        br: "{\\mathrm{BR}}",
        eqdef: "{\\,\\mathbin{\\stackrel{\\rm def}{=}} \\,}"
      },
      extensions: ["AMSmath.js", "AMSsymbols.js"]
    }
  });
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.6/latest.js?config=TeX-AMS_SVG">
</script>
```

Another feature of MaxJax I like is that MaxJax by default also supports cross references. I prefer using cross references in math delimiters, otherwise you have to use `\\` to specify a backslash, which is a pain.

If you are running a blog, a convenient way is to put this script in the file that is included in the `<head></head>` block of every html file.

For more details of the configuration, you can check the [documentation](http://docs.mathjax.org/en/latest/tex.html).

Update: MathJax now has updated to Version 3. 
