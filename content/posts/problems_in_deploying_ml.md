---
title: "Obstacles in Taking Machine Learning to Production"
date: 2017-07-25
tags: ["machine-learning", "data-science", "python"]
draft: false
---

Well the title is intentionally exaggerating. May not the biggest but certainly one of the most important area of friction in taking machine learning to production.

## Scenario

Imagine that you/your data scientist has written a functional machine learning pipeline in Python today. And by pipeline I mean data transformation as well as prediction code. For example, you could have a data which is mix of text and numeric features. You might do some text processing to generate n-gram features with some custom filters on the text. The text features are let us say concatenated with numeric features to form your independent variables/X matrix. Then let us say you are applying a non GLM machine learning model like trees or random forest. The story is good so far.

## Problem Statement

*   Problem 1: How do you use this pipeline for making a predictions later?
*   Problem 2: 1 + production forces you to use non-Python stack on application/server side.
*   Problem 3: 1, 2 combined alongwith the fact that production (software) engineer has never heard about feature engineering, random forest evaluation, training vs. prediction and so on.
*   Problem 4: how do you solve 1, 2, 3 and plus handle things like unit tests, regression testing, builds, logging, authentication seamlessly?
*   Probelm 5: data and business changes. How do you continuously train your models, version control them and still solve 1-4?

If you ask me, this scenario is certainly the biggest pain-point which prevents machine learning from actually delivering value to business. My experience of (trying to) solve this for over five years has taught some invaluable lessons.

## Solutions

Before dissecting the problem further, let us look at some naive solutions that one might look at (in increasing order of horror/tech investment).

1.  Use python mostly everywhere. Save pickled files to version controlled network file system and load on server side.
2.  Use an API/microservice/RPC layer to abstract Python implementation details (catch: writing that svc layer is only a first step, maintaining?).
3.  Use apache spark, engineers might feel more at home with JVM. (catch: too many to list down!)
4.  Use platform agnostic serialization like PMML (catch: coverage, library support, PMML didn’t really catch on?)
5.  Let engineers develop home grown solution to bridge the gap (catch: engineers need to learn lot of core ML).

## Dissecting the problem further

The problem looks simple to solve. But there are tons of problems that lurking beneath a naive solution here.

### Problem of Terminology / Common Language

In beautifully designed world of scikit-learn/Python certain basic operations are part of lingua franca. Vectorized text feature generation from a list of text or vectorized random forest prediction/evaluation is a basic operation. Having worked with Python for so long, data/ML oriented programming is ingrained in my brain so much. But other ecosystems that you use in production may not be mature on data/ML side to have these basic operations available as routine features. The result is that ‘vectorized text to n-gram features’ or ‘one-hot encoding’ is a greek for SDE building the production code.

For anything beyond a linear model, re-creating the prediction code in other language/eco-system is also tough.

### Problem of Re-inventing the wheel

You might try to isolate machine learning model prediction and feature generation in Python behind a service (or RPC). But then what about tons of other stuff like auth-handling, database communication, logging infrastructure, build pipelines which may or may not have been built in keeping multiple language support in mind? I’ve faced this problem where we loose productive time in re-inventing ‘XforPy’ before taking a model to production.

### Problem of (premature) Big Data Stack

Apache spark is a seducting alterative. It has Python, Scala, and Java API. So data scientist can train in Python, save the model. API layer can continue to use Scala or Java. Problem solved!

Not so fast. Though Apache Spark has its merits, we must understand that it was built with big data in mind. Hot data of a mid scale enterprise may still be too small for Spark/Hadoop. Secondly, Apache Spark is not a magick wand. You have to write the code to carefully exploit the parallelization primitives. Third, MLlib, although a great library, is still not as rich as Pydata/ML ecosystem.

## Solutions and Bottom Line

In my opinion, this is not a tool problem but a culture/education problem. (Data) engineers and data scientists have to understand each others trade to a good amount of depth and then choose their poison together (python everywhere or spark or something else). Companies like Google solve this problem since their cream engineers and cream machine learning researchers are the same (check Jeff Dean leading Google Brain, for example).

In my org, I designed a careful bootcamp for every new data science joinee where they learn about atleast basic software engineering practice like unit testing, build automation, version control and even good amount of practice on how Linux works. Sadly, teaching SDEs about ML basics has not happened.

Alternatively, libraries like dask offer a good trade-off where you can still stay in the Python eco-system and handle awkwardly sized non-big data well.

But as an industry, we are way off from ‘one way of doing things’ when it comes to actually deploying the prediction side of machine learning to production.

Though I hate to say it, the solution is ‘it depends’!

## Related Links

[In this article](http://tinyclouds.org/residency/) creator of node.js Rian Dahl puts it beautifully: “From a software maintenance perspective there is little consensus on how to organize ML projects. It feels like websites before Rails came out: a bunch of random PHP scripts with an unholy mixture of business logic and markup sprinkled throughout”, although in the context of training models and not scoring.

[Machine learning infrastructure at Stripe](https://www.youtube.com/watch?v=vKU8MWORHP8) Stripe built lot of prediction infrastructure with ‘train in Py use in Scala’ way. It is a good talk.
