---
title: "Practical Common Lisp / Clojure: 1"
date: 2017-05-31
tags: ["clojure", "lisp", "pcl"]
draft: false
---

In these posts I’m studying the book Practical Common Lisp by Peter Siebel and coding the examples in Clojure. Aim: studying clojure and reading this fantastic book [can be accessed online here](http://www.gigamonkeys.com/book/).

## Chapter 3 of the book

We try to implement a simple database for storing information about CDs that we own.

### CD & Records

First decision is how do you store the details for a single CD. Peter says that we (for now) want to store 4 properties about a CD. He introduces a simple list, but highlights that a property to value mapping will be more convinient than positional. Looks like common lisp has something called a plist for storing a symbol with a mapping to another value. plist also has getf function to fetch value given a symbol. Finally, he shows a constructor function for building data about a single cd by populating a plist.

In Clojure, there’s no plist to the best of my knowledge. But we have real hash table and I see no need for a poor man’s hash table as Peter calls it. Also, we use keywords for what’s called symbols in that section. Finally, in cl implementation using plist, probably the ordering of keys/properties is probably preserved. We use hash-map in clojure which won’t preseve key insertion ordering. We can probably use sorted-map in clojure, but I don’t think it makes any difference. So, the clojure implmentation of the examples is as follows,

    (hash-map :a 1 :b 2 :c 3)

    ;; getf in cl example is same as get in clojure

    (get (hash-map :a 1 :b 2 :c 3) :a)

    ;; so make-cd constructor can just return a map
    (defn make-cd
      [title artist rating ripped]
      (hash-map :title title :artist artist :rating rating :ripped ripped))

    ;; check on a single example
    (make-cd "Roses" "Kathy Mattea" 7 true)

### Filling CDs

The above section highlighted data rep for a single CD record. For storing data for multiple CDs, Peter advises using a simple list for now. The current value of the list is bound to a var and in each time we want to add a new cd, we push the new record into the list. In the book, Peter uses defvar macro to define _db_ as nil and then uses push macro to push new values. I don’t have enough knowledge of common lisp to know if this is same as declaring an empty list and then doing maybe a cons on it.

In the following code, I make two changes. Since we have a state (current state of database) which we want to repeatedly update, I am storing it in a Clojure atom. If I’m not wrong this is the usecase for which atoms are built. We don’t yet bother about transactions for our db where we may add multiple CDs at once. Secondly, in the absence of equivalent push macro, I am simply using a conj on the current value of the database/list. So the code looks like,

    ;; database current value is a list, state is an atom

    (def db (atom '()))

    ;; adding a new record -> add to the current state (list)
    (defn add-record
      [cd]
      (swap! db conj cd))

    ;; test on few examples
    (add-record (make-cd "Roses" "Kathy Mattea" 7 true))
    (add-record (make-cd "Fly" "Dixie Chicks" 8 true))
    (add-record (make-cd "Home" "Dixie Chicks" 9 true))

### Learnings So Far

Mostly standard clojure stuff. Probably first time I’m using atom even in a toy code. Peter has (between the lines) highlighted couple of good coding principles too which I originally read in the famous SICP book.

1.  Use data constructors and data accessors for data modelling (ch 2 of SICP book). Here we saw that action in make-cd function.
2.  Hide implementation details of data from user as far as possible. So by using add-record function, we have abstracted out how the database is implemented. If we swap out (pun intented!) the implementation from an atom holding list to something fancier, user wouldn’t have to bother, the add-record function should take care of migration to new implementation.

All in all, good start so far. In next sections, the real meat starts with Peter eventually implementing almost generic query parser on the database using macros. Looking forward to reading!
