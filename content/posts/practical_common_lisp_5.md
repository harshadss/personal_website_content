---
title: "Practical Common Lisp / Clojure: 5"
date: 2017-06-13
tags: ["clojure", "lisp", "pcl"]
draft: false
---

In these posts I’m studying the book Practical Common Lisp by Peter Seibel and coding the examples in Clojure. Aim: studying clojure and reading this fantastic book [can be accessed online here](http://www.gigamonkeys.com/book/).

In [part 4](/post/practical_common_lisp_4/) of this post, we saw the code from chapter 9 of the book about creating a test framework.

In this post, we continue the code of chapter 9 and iron out few kinks in our unit test framework.

## Chapter 9 Continued

### Note on combine-result

The code given in the book which uses combine-result is little different from the code I have written in previous post and this one. I’m skipping replicating the same functionality since my aim is to accelerate learning.

### Better Result Reporting

Whenever a test fails, we want to report which test failed with its name. The code is as follows (I’m modiying the report function). Note that I’m simply doing an AND over result of multiple test function rather than writing combine-results.

Note the trick with declaring a (dynamic) var test-name while defining report-result-return and how it is re-bound while defining the test function. I don’t yet understand why this trick is needed completely. My initial guess: probably difference between compile-time and run-time for the macro plays some role here.

    (def ^:dynamic *test-name* nil)

    (defn report-result-return
      [result form]
      (do 
        (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a: ~a~%" result *test-name* form)
        result))

    (defmacro multicheck-withres
      [& forms]
      `(every? identity (list ~@(for [form forms]
                                            `(report-result-return ~form '~form)))))

    (defn test-*
      []
      (binding [*test-name* 'test-*]
      (multicheck-withres
        (= (* 1 2) 2)
        (= (* 1 2 3) 6))))

    (test-*)

    (defn test-+
      []
      (binding [*test-name* 'test-+]
      (multicheck-withres
        (= (+ 1 2) 3)
        (= (+ 1 2 3) 6))))

    (test-+)

    (defn test-arithmatic
      []
      (and
       (test-+)
       (test-*)))

    (test-arithmatic)

### An Abstraction Emerges

We are still defining the test as a function, a detail leaked to end user. For each test function definition, we have the following fairly duplicated code,

1.  Define a function with the provided name
2.  rebind the test-name dynamic var to the name of the function
3.  Make a call to the check macro with the passed in forms.

We can remove all the code duplication and let the user define a test in a very concise way. The code is as follows,

    (defmacro defmytest
      [name & body]
      `(defn ~name []
        (binding [*test-name* '~name]
         (multicheck-withres ~@body))))

    (println (macroexpand-1 '(defmytest test-+ (= (+ 1 2) 3) (= (+ 1 2 3) 6))))

    (defmytest test-+ (= (+ 1 2) 3) (= (+ 1 2 3) 6))

    (test-+)

### Learning So Far

Good going so far. Multiple usage of macro function to simplify the code so much. The overall unit test framework code (with simplified names) is as follows (skipped the combine-result part). Short and sweet! I’d love to now checkout the source code of Clojure’s test frameworks like midje or core.test work.

    (def ^:dynamic *test-name* nil)

    (defn report-result
      [result form]
      (do 
        (clojure.pprint/cl-format true "~:[FAIL~;pass~] ... ~a: ~a~%" result *test-name* form)
        result))

    (defmacro check
      [& forms]
      `(every? identity (list ~@(for [form forms]
                                            `(report-result ~form '~form)))))

    (defmacro deftest
      [name & body]
      `(defn ~name []
        (binding [*test-name* '~name]
         (check ~@body))))

    (deftest test-+ (= (+ 1 2) 3) (= (+ 1 2 3) 6))

    (test-+)

Onwards!
