---
title: "Learning awk is useful!"
date: 2016-11-30
tags: ["awk", "unix"]
draft: true
---

I have been working with Unix (Linux to be precise) command line since a long time. But I had always avoided learning about Unix power tools like awk, sed. Quick exploratory analysis of data in a tabular/csv form is daily routine for me. My tool of choice for this work is either R or Python. I used to wonder about the utility of tool like awk. But recently I found myself using awk for some specific use cases. I was pleasantly surprised by productivity boost it can offer and its not so steep learning curve.

## Where is it useful?

My exploratory data analysis work has some common steps:

1.  Fetch data from the source (database, URLs)
2.  Do some quick analysis to make sure you have usable data.
3.  Iterate/change your data fetch queries based on inference from step 2.
4.  Optional step of download/scp the data to right server/local machine.
5.  Proceed with the real data analysis using R or Python.

I found awk to be a very useful tool for step 2 above. Many at times my source data is on a server in my organization which doesn’t have R installed. Let us say I have a huge file on the server which I plan to analyze and build a few models on. Before downloading the file locally, I need to check few things (example: if I am analyzing marketing funnel data, do I have particular funnel step present in the data?). In such use case awk is a handy tool to use.

## How to learn?

Learning awk through man pages or by looking at examples online is not useful. The best source is ‘The AWK programming language’ book by Kernighan et. al. Though the book 220 pages long, going through first 19 pages of tutorial is enough to make you productive in awk.

## Conclusion

For real, interactive exploratory data analysis work R and Python Scipy stack are far more powerful. But for specific use cases/jobs awk is useful. In true unix tradition, it is a tool that does these jobs well!

## Happy learning!
