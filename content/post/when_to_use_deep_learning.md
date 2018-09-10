---
title: "When (not) to use deep learning"
date: 2018-09-10
tags: ["machine learning", "deep learning"]
draft: false
---

I've documented my disdain for using 'deep learning as a hammer looking for nails in every corner'
in few other entries on this blog. Taking a less cynical and acerbic view, I want to focus on
when deep learning can be a good starting point and when it is not useful as starting point.

First to clarify bit of terminology. Deep learning is a catch-all term used in popular data science
these days. But we must separate out what this term encompases. It covers the following broad ideas,

  1. Novel neural network architectures tailored for specific applications (example: LSTMs in language)
  2. Innovations at algorithmic (example: adam optimizer) and component (example: GELU non-linearity) level
     for improving performance of optimization algorithms which find weights for neural network.
  3. Engineering discipline which tries to solve for problems in 1-2 at hardware (GPUs and TPUs) and
     coding abstractions (example: auto-grad, libraries) level.

This post is focussed on (1).


### When Deep Learning is a Bad Choice

  1. If the base representation of your data as seen by a computer and by a human is almost the same.
  2. When the data consists of mostly heterogeneous pieces of information.

For example, consider problem of predicting whether a customer will default on a loan or not. The data
will be a mix of demographics, details about the product offering (interest rates, amount), customer's
repayment history with same bank and so on. Most of the columns in such a dataset are measured on different
scale and they represent completely different things. For such problems, human beings cannot spot
decision rules or features just by looking at data. I've found that random-forest and gradient boosting methods
often give respectable and easy to use start on such problem. Note that I'm not arguing that random forest will
always outperform carefully tuned neural network applied to such problems. Rather, I'm saying that random-forest
kind of algorithms are safe choice which will give you good accuracy without much effort. Only after you've
established a good baseline with RFs and you are convinced that the baseline is absolute disaster should you
consider other ideas like deep learning. Deep learning may come into picture when your feature engineering can't
keep up with evolution of problem complexity. [Check out this post on fraud detection at Lyft for one example](https://eng.lyft.com/fingerprinting-fraudulent-behavior-6663d0264fad)


### When Deep Learning is a Good Choice

  1. If base representation of your data as seen by a computer and by a human is radically different.
  2. Data consists mostly of homogeneous pieces of information.

Consider images, a five year old child can routinely distinguish between a cat and a dog image. On the other
hand, a computer only sees bunch of pixels. The gap between representation and features is too large. Also,
the data is homogeneous: everything is an integer value representing intensity at pixel. Similarly, language
applications. To the computer, the base representation is just a sequence of unicodes. But we human beings
can quickly see patterns, meanings. Again the data is homogeneous (sequence of integers representing unicode
points) and gap between representation of information and meaning of information is too large. In such applications,
you effectively want the computer to learn to represent knowledge (for a limited domain) first, the classification
model is secondary. Deep learning seems to be excelling at such applications. There are few people who infact refer
to deep learning as representation learning.

Hopefully, this post gives a good thumb rule on applying deep learning.
