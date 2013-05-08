---
layout: post
title: "Nothing But Just Test"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Test png picture
![test png](/pictures/test.png)
![test png](/pictures/test.png?raw=true)   
png OK.

## Test svg picture
![test svg](/pictures/test.svg?raw=true)   
svg picture does not be supported!

## Test Pgyments
{% highlight scheme %}
(define (cons first second)
  (let* ((dispatch
          (lambda (command)
            (cond ((eq? command 'car) first)
                  ((eq? command 'cdr) second)))))
    dispatch))
(define (car c)
  (c 'car))
(define (cdr c)
  (c 'cdr))
{% endhighlight %}
                   
## Test MathJax
$$E = mc^2$$

\\\[ E = mc^2 \\\]

My favorite: \\\( E = mc^2 \\\) .


