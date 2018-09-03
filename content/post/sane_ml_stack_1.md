---
title: "Sane Choices for Small Machine Learning Teams: Part 1"
date: 2018-08-13
tags: ["machine learning", "deep learning", "python"]
draft: false
---

Data scientists and machine learning engineers in small and medium businesses often end up
over-engineering their machine learning workflow and stack. In a series of posts below,
I will share a few tricks learnt over the years related to choosing right components of the
ML pipeline.

In this first post, let us go through mistakes small teams can make. Later posts will explain possible solutions.


### Preferring Generalized Solutions

Many big companies will perfect a very general solution to machine learning problems by investing
obscene amount of resources. For example, Google and Facebook can employ top notch PhDs in the
field of machine learning and let them work on some complex problem for months. These companies can
also invest in expensive technology (costly GPU time). Big companies like solving a class of problems
by employing very generic solutions: Google can say that everything is a neural network problem if you squint
hard enough. So they will build things like tensorflow and use neural nets everywhere. Once problems are
solved in a general manner, they can be optimized better. Specialized teams can optimize last drop of efficiencies
from models, architectures or hardware. But the benefits from specialization are enjoyed by all teams.

But as a small startup / ML team, generalizing solutions will be your enemy number 1. You are far better off
in solving specific problem at hand as quickly and cheaply as possible.


### Optimizing for Wrong Metric

Big companies can actually afford to run machine learning/AI practice in two stages. One is a reasearch stage where
employees spend a lot of time on a complex problem and solve it very well. Second is the engineering/product integration
stage. This is possible because their return on investment metric is very different from a small startup.

Let us take example of Google's smart replies. If you read their research paper, few things stand out. Almost 8-9 people
are mentioned as authors with 4 as equal contributors. So that means atleast 4 people worked on the core of this problem
for a long time. Secondly, their final solution is not one single deep learning algorithm but a complex solution consisting of 4 stages atleast
one of which had human intervention. Third is data: their model had access to 230 million labelled messages for training
a model. But Google can justify such a gigantic effort. If smart replies result in even 10% more mails sent on mobile, it is
a massive return for Google (more engagement with app, more ads). Same is the case with other companies like OpenAI. If you read their
paper on unsupervised language modelling, you can see tons of hardwork and experiments around right architecture and integrating
very recent innovations like GeLU non-linearities. Big companies optimize for technological breakthroughs on broad class of problems.

The ROI metric for smaller companies/machine learning teams are very different. You might have 2-3 (albeit smart) people wearing
many hats: ML researcher, production ML engineer and ML ops engineer are problably the same person in your team. A 5% increase in accuracy
of a classifier might mean billions dollars for Google but nothing for your team. So small teams should spend more time carefully
picking specific problems worth solving and optimizing for time to market.


### Thinking in Solution Space

Imagine a small/medium company grappling with losses due to poor visibility about product demand and you are solving their problem
using machine learning. Thinking in solution space first means thinking "I want to build most accurate forecasting algorithm for this data".
Instead, prefer to think in terms of problem space: "I want to minimize losses by better estimating demand for this product". Thinking
in solution space makes you optimize for a narrow and probably less relevant metric for the business.


### Feeling Ashamed of Feature Engineering

On specific set of problems like computer vision and language, deep learning far outperforms handmade features. But again, these benefits
are not universal (yet). For example: rather than throwing huge LSTMs at a time series problem, you are better off with carefully feature
engineered (generalized) linear regression. Research driven by big companies creates this false narrative: human curated features are
bad, make everything a giant neural network black box.

As a small startup, the knowledge your team has about a specific domain is your advantage. Your models can learn something faster (days)
compared to a team of PhDs at Google can take months to learn.


### Uninformed or Subjective Choice of Tools

At small-to-medium data and business scales, the tools that you choose are going to matter a lot. Choosing a wrong machine learning
library (scikit-learn Vs. tensorflow), tech architecture (monolith Vs. microservices) can result in delays in releasing your solution.
I'm not advocating for shunning libraries from big companies. But the trade-offs for tools need to be evaluated more objectively. If
you need to run image classification inference on low powered devices, tensorflow and its ecosystem is probably best bet at the moment.
However, for another class of problems and business domain, you might do a lot better using scikit-learn on a beefy server. Avoid at all
costs the only criteria for selection of tool as "it is released by a big company so it must be good".


### Bottomline: Think Like a Guerrilla

The overreaching theme of above explanation is this: small machine learning teams and companies need to think like a guerrilla than
officer of a sophisticated army. Here are few techniques a small machine learning can utilize for maximizing chance of success. I
will expand on each choice in separate posts.

  1. Survive first: your feature, product first needs to go in the market and start making money. Your ML workflow and stack can
     evolve into better shape much later.
  2. Choose boring technology: regression models, random forests have been around for a long time. So is scipy and scikit-learn.
     You are far less likely to run into lack of documentation, bad APIs and bugs with these algorithms and libraries.
  3. Pirate spirit: take whatever you can! Once in a while, big companies will release a solution that is objectively better at
     some tasks (think word2vec or pre-trained NNs for images). In such case, make maximum use of it.
  4. Play at your strength: knowledge of specific business domain, expertise with older libraries might be strength of your team.
     Play with your strengths.
  5. Avoid distributed solutions when you can: one beefy server with 64GBs RAM beats debugging 1000 line Java stack-traces in Apache
     Spark.


Big companies prefer creating monopolies because they can profit from it. They earlier settled for one social network and one search
engine. The next step is one algorithm, one library, one cloud management solution. These companies are *not* working for
commoditizing machine learning. Commoditization means perfect competition in a market: large number of players offering undifferentiated
solutions. Google, FB and MS are actually battling for the other end: one probably misfit and costly solution for everyone.



