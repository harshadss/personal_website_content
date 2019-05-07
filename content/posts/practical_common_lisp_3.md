---
title: "Practical Common Lisp / Clojure: 3"
date: 2017-06-08
tags: ["clojure", "lisp", "pcl"]
draft: false
---

<p>In these posts I&rsquo;m studying the book Practical Common Lisp by Peter Siebel and
coding the examples in Clojure. Aim: studying clojure and reading this
fantastic book <a href="http://www.gigamonkeys.com/book/" target="_blank">can be accessed online here</a>.</p>

<p>In <a href="/post/practical_common_lisp_2/" target="_blank">part 2</a> of this post, we
implemented select, where and update function for our documentish db. But the
implementation had fair bit of code duplication. We remove code duplication
and make the code more generic in this section.</p>

<h2 id="chapter-3-continued">Chapter 3 continued</h2>

<h3 id="removing-duplication">Removing duplication</h3>

<p>In this section, Peter highlights that last sections where implementation was
specific to the given use case (documents corresponding to cd) and had fair bit
of code repeatation. He solves both problems with a re-write of where as a macro
with two helper functions.</p>

<p>My clojure code is as given below, the explanation follows.</p>

<h4 id="single-comparison-expression">Single comparison expression</h4>

<p>In a database query, there could be multiple where clauses/conditions.
For each clause, we would need a comparison expression. The following code
generates such expression for a passed in field and value. For reasons little
difficult for me to digest right now, there&rsquo;s a reference to a dynamic var row
in the implementation. We will discuss this point later. Note for now that the
function gives different result a new binding of row var.</p>

<pre><code>(def ^:dynamic row nil)

(defn make-comparison-expr
  [[field value]]
  (list '= value (list 'get row field)))

(make-comparison-expr '(:rating 8))

(binding [row {:rating 8 :artist &quot;Someone&quot;}]
  (make-comparison-expr '(:rating 8)))
  
</code></pre>

<h4 id="multiple-comparison-expressions">Multiple comparison expressions</h4>

<p>What if there are multiple clauses/conditions in where? We iterate over each
pair and build a list containing each comparison expression. Since the passed
in list is a sequence of field and value, we use partition to retrieve pairs.</p>

<pre><code>;; for multiple pairs, make expression for each
(defn make-comparison-expr-list
  [all-field-pairs]
  (let [pairs (partition 2 all-field-pairs)]
    (map #(make-comparison-expr %) pairs)))
    
(binding [row {:rating 8 :artist 9}]
  (make-comparison-expr-list '(:artist &quot;Dixie Chicks&quot; :rating 8)))

</code></pre>

<h4 id="the-where-macro">The where macro</h4>

<p>Now comes the hairy part. Here we want to write where as macro, which returns
the right predicate generator. To be honest, I struggled with this part a lot.
There are multiple reasons,</p>

<ol>
<li>The macro is returning an anonymous function. The anonymous function takes
 an argument (the row/document in the database). Not covered in many macro
 tutorials/training :-P</li>
<li>The and in Clojure itself is a macro which takes expressions and not a list
 of expressions. Whereas our make-comparison-expr-list is returning a list.
 We can use ~@ to splice the list, but I am staying clear of it for now.</li>
<li>The reference to the row needed for make-comparison-expr. I thought the best
 way would be to pass in a symbol. So make-comparison-expr function should
 also take in row alongwith field, value pair. I couldn&rsquo;t get this to work.</li>
</ol>

<p>Although I could write the two functions in the last subsection myself, the
macro simply too hard for me at this point. So I needed some help from
<a href="https://github.com/stuarthalloway/practical-cl-clojure/blob/master/src/pcl/chap_03.clj" target="_blank">Stuart Halloway&rsquo;s solution here</a>.
I solved problem by appending a pound/hash to the row
argument to the anonymous function. My incomplete understanding tells me that
it generates a new symbol for the anonymous function argument. Variable capture
and what not. I need to study this later.
For problem 2, I used every? function with eval on each return value from
make-comparison-expr-list rather than splice unquote. For problem 3,
I still don&rsquo;t understand why we need a global reference to the row which is
bound again inside the macro. I do know that getting the macro to work is
littled difficult otherwise.</p>

<pre><code>(defmacro where [clauses]
  `(fn [cd#]
     (binding [row cd#]
      (every? eval (make-comparison-expr-list ~clauses)))))

(defn select [wherefunc]
  (filter wherefunc (deref db)))

(select (where '(:title &quot;Fly&quot;)))

</code></pre>

<h3 id="learning-so-far">Learning So Far</h3>

<ol>
<li>Treating code as data, wow! We generated the expression for comparison
 depending upon the field and value passed in. I&rsquo;ve not seen this done
 in any other language so succinctly.</li>
<li>Treating code as data helped us to make the functions more generic.
 <a href="https://www.cs.umd.edu/~nau/cmsc421/norvig-lisp-style.pdf" target="_blank">Peter Norvig&rsquo;s lisp guide</a>
 highlighted this explicitly and so did SICP book: make your functions
 generic so that they can be re-used. Saw this in action.</li>
<li>First rule of macro club is there for a reason! Stay clear of macros as long
 as you don&rsquo;t understand them well.</li>
</ol>

<p>This concludes chapter 3. Onwards!</p>
