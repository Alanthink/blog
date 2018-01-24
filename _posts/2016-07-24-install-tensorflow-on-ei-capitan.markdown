---
excerpt: ""
comments:	true
share:	false
title: Install Tensorflow on Ei Capitan
date: 2016-07-24 09:22:54 +0000
categories:
  - cs
tags:
  - tensorflow 
  - ei capitan
---

## Update numpy

In order to install Tensorflow on EI Capitan, numpy needs to  be updated. However, numpy was installed in `/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python` which can not be modified even with root privilege. In EI Capitan, this mechanism is called __SIP (System Integrity Protection)__. Generally, the following folders can not be modified. 

* `/System`


* `/usr`


* `/bin`


* `/sbin`


* Apps that are pre-installed with OS X

My solution is firstly to disable SIP and then update numpy. This solution is __risky__ as disabling SIP is an operation that I am not familiar with.

### disable SIP

1. restart computer and press `commend+R` to enter recovery mode
2. open terminal and run the following command

```shell
$ csrutil disable
```

### update numpy

Here I download numpy package first and then use pip to install it. What's more, as default installation path for pip is `/Library/Python/2.7/site-packages` which is not what I expect. Therefore, I use `—target` to specify the path within which I want to install.

```shell
$ sudo pip install --upgrade --target=/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/ $(numpy_package_path)
```

## Install Tensorflow

Again, I download tensorflow package first.

```shell
$ sudo pip install --target=/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/ $(tensorflow_package_path)
```

