---
layout: post
title: "389ds alias plugin"
date: 2023-03-16
tags: 389ds plugin alias
excerpt_separator: <!--more-->
---

![kdpv]({{ "/assets/flachau_1200x350.jpg" | relative_url }})

The dereference of aliases is unfortunately [not supported](https://github.com/389ds/389-ds-base/issues/152) by the [389ds ldap server](https://www.port389.org/). Therefore I've created a small plugin which resolves aliases during **base** search. Subtree and onelevel searches are not supported.

It is [here](https://github.com/anilech/alias-base)
<!--more-->
