---
title: "Skeptics Guide To ML Hype and Excitement"
date: 2019-10-15T15:19:02+05:30
draft: false
toc: false
images:
tags: ["machine-learning", "deep-learning"]
---

Machine learning field at the moment seems to be in pinnacle of hype cycle.
A researcher or a lab or organization creates some marginal improvement over
existing status quo in some narrow area, they self-publish it as paper with
some preposturous headline and then the circle jerk starts.

I have a personal heuristics to deal with such hype cycle based on following
observations.

## Observations

### Not All Papers are Created Equal

In most of the universities it is mandatory for a master's and PhD student to
publish some work. Even academics have to keep playing the reputation-via-citations
game. Based on older data that I had access to [about CS PhDs in US](https://www.nsf.gov/statistics/2016/nsf16300/digest/)
united states alone produces ~20,000 master's and PhD students in a year in computer
science. The number for china will be similar (or more) I assume. India will not be
on such high number but neither will it be insignificant.

A large percentage of these students will be doing their thesis in whatever is hot
in the market right now (ML!). So at any given day, a large mass of machine learning
research papers will be produced. The reaction of media, clueless students and
[mops](https://meaningness.com/geeks-mops-sociopaths) is of constant excitement
over every such news and publication.

It is important to realize that not all of these papers are bringing any fundamental
advance to the field. Most of them will be doing some marginal improvement in a
specific (more often impractical) area. The reproducibility of many papers is
questionable. Since academics use published literature (vs say something like
creation of scientific software) as important measure to be improved, [Goodhart's law](https://en.wikipedia.org/wiki/Goodhart%27s_law)
applies!

Once in a while a master's student will publish something great (example: attention
models) but this is rare event, and not norm.

### Transformative Research is Rare

If you look back on any area of computer science (or any science in general), transformative
research which changes a field is published/comes to mainstream once in a decade maybe. The
reason is that it takes immense amount of time and a dogged researcher who keeps working on
something that s/he fundamentally believes in. [Hinton was working on distributed representations in 1986](http://www.cs.toronto.edu/~hinton/absps/pdp8.pdf). People considered [John Hopfield a little bit crazy for working on
neural nets in 1983](http://longnow.org/essays/richard-feynman-connection-machine/). Just like
fortunes are made in stock market over decades (but can be lost overnight), it is statistically improbable
that some fundamental innovation in machine learning will keep on happening every month consistently.

### Selling the Shovels in Goldrush

In financial markets and investments, stock brokers profit no matter which way stock market
goes. In any field that resembles a gold rush, the people selling shovels are always going
to say that there's gold everywhere, all you need to do is buy their shovel and start mindlessly
digging.

## Heuristics

Based on the above observations, I follow this heuristics,

  1. Mild skepticism should be the default response to any new excitement. In fact that should
  be a pre-requisite for calling yourself a (data) scientist!
  2. Don't use mainstream media to follow research on ML. If it is in mainstream news, it is
  old stuff that charlatans are waking upto or a PR stunt.
  3. Look out for researchers who have been doing research in certain are of ML for decades and
  follow their students work instead (example: Bengio).
  4. Don't take advise from cloud players or infrastructure people. They are equivalent of stock
  brokers giving tips.
  5. Look out for contrarians who stick their neck out even when people ridicule them. For example,
  [Judea Pearl has been saying that deep learning won't give you AI since some time](https://www.quantamagazine.org/to-build-truly-intelligent-machines-teach-them-cause-and-effect-20180515/).
  6. Read older papers in the field (LSTM paper is from 1997, for example!) and ask yourself: which
  of the assumptions or constraints from the paper can be pushed today?

## Shoeshine Boy Who Gave Stock Tips

There's an (apocryphal?) story about [a rich investor who exited stock market before a crash](https://archive.fortune.com/magazines/fortune/fortune_archive/1996/04/15/211503/index.htm) because his shoeshine boy started giving him stock tips.
Probably I am paranoid but machine learning field resembles such crazy levels to me at the moment.
In my hometown Pune, a generally conservative city which is not at forefront of tech innovation (compared
to US Bay Area or even Bangalore), data-science training institutes have mushroomed in every nook and corner.
Often these are run by shady teachers with questionable knowledge of the field. I am not selling off,
but definitely keeping a close eye on proceedings. I am also diversifying my knowledge into older and
not-in-hype-cycle fields of applied mathematics.

