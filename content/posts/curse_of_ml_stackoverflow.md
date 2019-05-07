---
title: "Curse of ML and DL on Stack Exchange"
date: 2018-07-30
tags: ["machine learning", "data science", "deep learning"]
draft: false
---

I have come across two distinct flavours of beginner behaviours on stack-exchange site related to data science. I believe they are symptoms representing what is the larger
problem ailing field of data science. Let us go through the symptoms first. My tone in
this article is extremely acerbic because I feel strongly for my field.

### Deep Learning Voodo Problem

Someone will post a problem like, "I am trying to fit (some complex deep learning architecture) model
on (a bad problem for starting with DL). But nothing happens". The model is expected to magically
solve the problem but something utterly trivial happens. The model doesn't learn anything
or horribly overfits. Most often, people are doing something too ghastly for words like trying to
predict the price of a particular stock using DL hoping for a miracle.

The disease here is thinking that deep learning is simple and since Google/FB have demonstrated its
success in solving hard problems, one can replicate the results in a few minutes piggybacking on some
library. [Ali Rahimi was correct when he said that the field is turning into a practice of alchemy and
not science](https://www.youtube.com/watch?v=Qi1Yry33TQE).

Beginners are making following mistakes/assumptions,

  1. Since DL can solve hard problems, solving hard problems with DL would be easy.
  2. DL solves hard problems, so we should through linear models or any other classic ML model
     and jump to using DL on everything. Building baselines using simpler models is for sissies.
  3. Picking an utterly complex real-world problem with heterogeneous data with multiple co-variates (stock prices)
     and throwing a DL model which works on a simpler problem with homogeneous data (images) at it. In fact, in
     my opinion throwing any model at stock price prediction is a lunacy but I digress.

This is a sad state of things and it might cause the downfall of deep learning (which is a great fine field by the
way).

### Build My Product for Free Problem

Someone will post a problem like "I have a bunch of documents here and I want to achieve X. How to do it?". It
will be clear from the question that the person posting the question has no idea about even most fundamental
ideas in machine learning like vector space model. They are thinking that it is just a matter of plumbing things
correctly. 

It is not lack of machine learning skills (perfectly legit) but lack of appreciation about hard work
required to solve a business problem. It is perfectly alright to be a beginner in a field. We all were at some point in time. The problem here is company executives and founders who recruit a kid who has done a two-week deep learning Bootcamp and throw some complex business problem which will need careful modelling, engineering and inference at him/her. They think that
machine (deep) learning will magically make their team or startup successful. No a**holes, you can't expect to build a good cancer hospital with only first-year medical students (or plumbers) to make it successful. Like [fat tony would say](http://www.kitco.com/ind/Turk/turk_sep092008.html) building a CRUD app using JAVA and solving a business problem using machine learning is
not the same ting! Technical managers or startup founders are looking to make quick bucks using ML on some hard
problem. The result is many beginner data scientists on ds.stackexchange looking for the entire solution to the problem as
a free answer.

### The Abyss

The bar for any technical field is supposed to be higher. I am not suggesting that newcomers are not welcome, they
are. I go out of my way to teach and mentor dozens of newcomers in machine learning field. The more people that acquire modelling and statistical skills, less stupid the world becomes. But big corporations who are lying about machine learning application
being a commodity and media/journalists who half understand it before joining the bandwagon are ruining the field.
You become a data scientist or scientist of any kind by learning basic principles, carefully applying them to increasingly complex problems over years. There are no shortcuts to the hard work, no matter how many GPU boxes with tensorflow you throw at it.
