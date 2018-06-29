---
title: "Practical Common Lisp / Clojure: 4"
date: 2017-06-09
tags: ["clojure", "lisp", "pcl"]
draft: true
---

In these posts I’m studying the book Practical Common Lisp by Peter Seibel and coding the examples in Clojure. Aim: studying clojure and reading this fantastic book [can be accessed online here](http://www.gigamonkeys.com/book/).

In [part 3](/post/practical_common_lisp_3/) of this post, we saw the code from chapter 3 of the book in which we implemented an in-memory documentish database with select, where and update.

Now I am tackling the code from chapter 9, building a test framework. I’m skipping the chapters from 4-8 since they cover CL syntax and semantics from blog posts. But note that these chapters are not to be skipped while studying, especially the macro ones.

## Chapter 9 : Unit Test Framework

In this chapter Peter makes us implement a simple unit test framework for lisp with lot of macro kung-foo. The unit test framework is to be used in interactive fashion.

### First Try

Each test is defined as a good old function returning bool. It does and over multiple clauses/tests. Clojure code is simple,

    (defn test-+
      []
      (and
       (= (+ 1 2) 3)
       (= (+ 1 2 4) 7)
       (= (+ -1 -3) -3)))

    (test-+)

    (defn test-+
      []
      (and
       (= (+ 1 2) 3)
       (= (+ 1 2 4) 7)
       (= (+ -1 -3) -4)))

    (test-+)

However, this returns an all or nothing result but doesn’t help us to narrow down on which test case failed. So instead, we modify the function to print the input data and result for each clause/test. At this point the code takes an uncomfortable turn: reading about formatting function.

This leaves me with two choices either learn about common lisp formatting in detail or use Clojure formatting (which uses Java.util.Formatter). I’m already having hard time in life learning about different formatting in Python 2 and 3 :-P. So having to choose between cl formatting and clj/java is like between a rock and hard place, if you get my drift.

Anticlimax: Clojure comes to rescue as it can understand common lisp formatting. So onwards, we piggyback on cl-format in clojure and implement the test as follows.

    (defn test-+ []
      (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a~%" (= (+ 1 2) 3) '(= (+ 1 2) 3))
      (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a~%" (= (+ 1 2 3) 6) '(= (+ 1 2 3) 6))
      (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a~%" (= (+ -1 -3) -4) '(= (+ -1 -3) -4)))

    (test-+)

### Refactoring / Better Reporting

We note that the code above has some duplication as we are printing things repeatitively. We abstract out the printing part in a single function.

    (defn report-result
      [result form]
      (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a~%" result form))

    (defn test-+
      []
      (report-result (= (+ 1 2) 3) '(= (+ 1 2) 3)))

    (test-+)

But this is still not satisfactory, as the list representation and the expression are both passed to report-result. Smarter code should be able to generate the both based on one.

In the macro below, we generate the call to report result with the form. The ‘~ is little cryptic. The backtick ` converts all the atoms in the passed in list into namespaced symbols. But we don’t want that to happen with form (since it is not defined in the namespace, we want the actual form passed in to the macro). ~form forces the evaluation of form rather than the form itself.

    (defmacro check
      [form]
      `(report-result ~form '~form))

    (macroexpand-1 '(check (= (+ 1 2) 3)))

    (check (= (+ 1 2) 3))

With this, the test can be multiple calls to check.

    (defn test-+
      []
      (check (= (+ 1 2) 3) )
      (check (= (+ 1 2 3) 6))
      (check (= (+ -1 -3) -4)))

    (test-+)

That doesn’t look too bad, although there’s duplication with multiple calls to check. One more step further: let us auto-generate the multiple calls. Macros to the rescue again. Note that we use do in clojure for doing multiple form evaluation (progn is used in the book). For in Clojure is more readable too. Lastly, we change the name to say multicheck for easy comparison with check.

    (defmacro multicheck
      [& forms]
      `(do ~@(for [form forms]
               `(report-result ~form '~form))))

    (macroexpand-1 '(multicheck (= (+ 1 2) 3) (= (+ 1 2 3) 6)))

    (multicheck (= (+ 1 2) 3) (= (+ 1 2 3) 6))

    (defn test-+
      []
      (multicheck
        (= (+ 1 2) 3)
        (= (+ 1 2 3) 6)))

    (test-+)

### Fixing Return Value

The last iteration is not bad. The test (function really) looks more like the way would write real tests, with just enough details passed in. But we want to bring back our original feature: printing overall result (all tests passed or atleast one failed). The last implementation prints pass fail for each test but not overall result.

The idea is simple: while printing result of each test, we need to somehow capture the result of evaluation and then AND over all results and print the result of that AND.

First thought that comes to mind is AND in the language. But AND in CL/CLJ short circuits (breaks on first false). Going off track, it is interesting to read up why it makes sense in case of boolean logic to short circuit AND. But let us not digress.

Peter implements the refactoring with the help of two macros. I wanted to avoid that territory (macro calling another macro) given my shoddy knowledge of macros. So I am simplifying the implementation a little.

We change the report-result function to do the side effect as well as result. The ‘do’ will print to stdout and then return the result. For doing non short-circuit AND over result, Clojure has every? implemented for us. We splice the result of applying report-result and then convert to list again. Otherwise, we end up in a situation with s-expression with boolean as first atom, which is illegal. There’s probably a cleaner way to implement this. Some other time! Finally, we print the result of every.

    (defn report-result-return
      [result form]
      (do 
        (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a~%" result form)
        result))

    (report-result-return (= (+ 1 2) 3) '(= (+ 1 2) 3))

    (defmacro multicheck-withres
      [& forms]
      `(println (every? identity (list ~@(for [form forms]
                                            `(report-result-return ~form '~form))))))

    (println  (macroexpand-1 '(multicheck-withres (= (+ 1 2) 3) (= (+ 1 2 3) 6))))

    (multicheck-withres (= (+ 1 2) 3) (= (+ 1 2 3) 7))

    (defn test-+
      []
      (multicheck-withres
        (= (+ 1 2) 3)
        (= (+ 1 2 3) 6)))

    (test-+)

### Learning So Far

This chapter has been interesting to say the least. Multiple learnings as follows,

1.  More practice on macros. Remember that you take input form and return another form (list) which is actually evaluated.
2.  For use cases like this, lisps are in general unbeatable. For example, unit test framework is about generating code to execute supplied code and then doing bookeeping on what failed and what not. Can’t beat lisp with its code as data and macros here.
3.  Iterative implementation with the help of repl is such a liberating way to develop code. Short cycle between implement and testing helps.
4.  Peter’s iterative implementation also moves from a crude design to a better design. Progressively, we are only supplying required details (the test clauses) and not caring about how the implementation/evaluation of tests is done.

The last part where implementation is leaking to the user is in defining test as function and call to check. As a user you are aware that check needs to be called. Ideal design would,

1.  Hide that defn and let user define a test (deftest). It is like creating a mini DSL.
2.  User shouldn’t need to know if check is called internally.
3.  Ability for defining hierarchy of tests.

Peter tackles it in next part. Hint: it is more macros to generate the defn directly :-) . I can’t wait to read the next part!
