---
title: "Notes   Hugo"
date: 2020-07-27T15:01:37-07:00
categories:
 - Tech
 - Web
tags:
 - Hugo
draft: false
---

### How to pin a post at the top of the home page?
Use `weight` in front matter for ordering your content in lists. 
Lower weight gets higher precedence. 
So content with lower weight will come first. 
If set, weights should be non-zero, as 0 is interpreted as an unset weight.
