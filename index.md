---
layout: page
title: Jks PhiloSophy
tagline: WELCOME
---

#### Who I am
- A Chinese
- A programmer

#### Who I am not
- A computer repairman

#### Which language I use
- Chinese, English
- C C++, Emacs lisp, Racket

#### Which OS I use
- Linux/Ubuntu RedHat
- Win7 WinXp

### Posts: 
{% for post in site.posts reversed %}
####   [{{post.title}}]({{site.url}}{{post.url}})
{% endfor %}


