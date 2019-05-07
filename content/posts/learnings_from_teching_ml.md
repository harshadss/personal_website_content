---
title: "Learnings from Teaching Machine Learning"
date: 2017-12-19
tags: ["machine-learning", "data-science", "python"]
draft: false
---

I have been teaching machine learning to programmers since some time. I started this activity in 2013 and till date I have conducted 10+ hands on workshops.

All these workshops were typically 3-3.5 hours long and covered some theory, coding examples through Python (ocassionally R) and interactive discussions. They were attended by 10-50 programmers.

Here are some learnings from this teaching activity, in no particular order. Many of them were in fact goof-ups that I did at one point of time, so these are indeed lessons from trenches.

### On Preparation

1.  Never underestimate value of solid preparation. Doing machine learning (or any other domain really) for a long time is no excuse for not preparing. My thumb-rule is typically 5x-8x preparation i.e. for a 3 hours workshop, you need 20 hours of explicit preparation.
2.  Before you begin preparation, write down profile of your likely audience. For example, ask yourself how much your audience would know about a particular concept (say linear regression).
3.  Always plan for 2-3 different levels (or categories) of audience on the basis of familiarity with the topic and motivation.

### On Presentation

1.  Work out all your content on pen and paper first. Visualize the flow of your presentation. Start putting together slides at very end.
2.  If your presentation uses code, prefer creating slides in a tool which can let you integrate between content and code easily (example: ipython notebook in presentation mode). Context switch between code and content is not required.
3.  Pay attention to minor details like font size, line width. Try out how the presentation looks on projected screens, especially for slides with code.
4.  People have different preferred ways of learning: listening, discussing, coding it up, seeing in pictures and so on. Have a balanced mix of these teaching aids in your presentation.

### Content

1.  Write down all the content that you want to cover. Take a deep breath in. Delete half of the planned content. Seriously. Due to familarity with the content, you would always underestimate the time required to cover it by a large margin. Err on the side of covering fewer concepts. In the worst case you will have more time for question-answers, rather than running through slides and apologizing profusely.
2.  Assume that your audience is allergic to jargon. So number of new words, concepts that you throw at them should be minimized.
3.  Create enough time checks around big concepts/themes of your workshop. For example, for a 3 hour workshop I have time checks in mind for every hour about which concepts I should have covered by that time.
4.  If you are going to give analogies from other domains, review your analogies from perspective of audience. If most of your audience is engineers from computer science, analogies from communications engineering will not help. Same is the case with analogies involving places, people, currencies and festivals. Localization of your analogies and examples is important in order to avoid distractions.

### Tools and Environment and Testing

1.  Do not assume that your favourite editor, operating systems, programming language, notebook and any other tool is an industry standard.
2.  Focus on generic tools as much as possible to handle diversity of tools and OSses. (pip instead of apt-get, simple text editors, no IDE support). If you want to make assumptions, make lowest possible assumptions. For example, never assume that everyone in the audience knows about Docker or Conda.
3.  Create a well defined document on requirements and installation. Make sure that your audience has access to this document well in advance.
4.  A sizeable number of people might be using a non-unix operating system, plan for that.
5.  If you have code to be covered as a part of workshop, ensure that it is well tested. In fact having your audience run tests with you is also a good idea.
6.  Test your code examples from typical goof-ups (someone in the audience running a different version of a library, Python 2 Vs 3).
7.  Your presentation or workshop is not the right place to pitch your favourite programming language, editor, operating system (or _anything_ really) to the audience.
8.  It is good idea to put your code on github (or similar) in advance and share a link.
9.  Never assume that Wi-Fi at the workshop or conference venue will work flawlessly :-)  

Bottomline is that your audience invest significant time in coming for your workshop/presentations. It is your responsibility to ensure that they get maximum return on their time and money investments, without too many distractions.
