---
layout: post
title: "Standards And Protocols"
description: ""
category: Essay 
tags: []
---
{% include JB/setup %}

## Standards
A few days ago, I heard about Haskell program language. It's wonderful, so I begin to study it. I write a Hello word:

{% highlight haskell %}
#! /usr/bin/env runhaskell
main = putStrLn("Hello world.")
{% endhighlight %}

Then I runed it:
{% highlight bash %}
$ ./hello-world.hs
Hello world.
{% endhighlight %}

### Test
Every is OK here. But a friend of one of my friend(Michael Feathers) told me:
> [To me, legacy code is simply code without tests.](http://hackerboss.com/legacy-code/)

So I write a C program to check if the stand out is "Hello world.".

"Oh, It's beautiful!" I can't help but exclaimed. 

### Version Control
"You must make you code under a version control system!"

OMG, I almost forgot it.
{% highlight bash %}
$ git init 
$ git add .
$ git commit -m "Add init file hello-world.hs"
{% endhighlight %}

### Make tags
When using a version control system, you will always make a tag.
`$ git tag v1.0.0.0`.
No, no, no, you should use:
`$ git tag v1.0.0`.

I just don't know which one to use. Maybe I shoule use 
`$ git tag v1.0.0.0-alpha`, OR
`$ git tag v1.0.0-beta`.

Is there a standard? Just google it.

Maybe [semantic version](http://semver.org/) is what I want.

So, I typed `$ git tag v1.0.0+1 -a -m "Using semver"`, 
and echo these to my README.md:
{% highlight markdown %}
## About Version
This work will be compatible with [semantic version](http://semver.org/).
{% endhighlight %}

### RFC2119
"What is `will'?"

