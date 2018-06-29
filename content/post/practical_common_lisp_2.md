---
title: "Practical Common Lisp / Clojure: 2"
date: 2017-06-05
tags: ["clojure", "lisp", "pcl"]
draft: true
---

In these posts I’m studying the book Practical Common Lisp by Peter Siebel and coding the examples in Clojure. Aim: studying clojure and reading this fantastic book [can be accessed online here](http://www.gigamonkeys.com/book/).

In [part 1](/post/practical_common_lisp_1/) of this post, we went through creating a single database using list and maps.

## Chapter 3 continued

### Looking at database contents

In this section, Peter teaches string formatting for CL. Using string formatting, a nicer way to look at db contents is tried. Not very interesting to me for now. So we simply cheat as follows using clojure’s pprint. Remember that our db is modelled using clojure’s atom holding a list.

    (defn dump-db
      []
      (pprint (deref db)))

### Improving user interaction

Giving this section a pass too since I want to move to the juicer part faster.

### Saving and loading

Clojure’s pr function helps us here. It writes the objects in such a way that they can be read back later.

    (doc pr)

    (defn save-db [filename]
      (let [current-db (deref db)]
        (with-open [w (clojure.java.io/writer filename)]
          (binding [*out* w]
            (pr current-db)))))

### Querying the database

Now we come to the juicy part :-). Peter introduces multiple concepts here. One by one we learn,

1.  introduction to higher order functions through remove-if-not
2.  annonymous function through lambda
3.  keyword parameters to functions and optional arguments  

The result of using all the above concepts is where higher order function which can be used in combination with select to show matching records from the db. I’m implementing them in clojure as follows (there could be better ways). We use filter in clj instead of remove-if-not.

    ;; generate the right filter condition
    ;; note that for parameter not passed, we always return true so if works out.
    ;; where function returns a predicate, where is used with select

    (defn where
      [& {:keys [artist rating title] 
          :or [artist false rating false title false]}]
      (fn [cd]
        (and
         (if artist (= artist (get cd :artist)) true)
         (if rating (= rating (get cd :rating)) true)
         (if title (= title (get cd :title)) true))))

    ;; magic being in where, select function is straightforward

    (defn select [wherefunc]
      (filter wherefunc (deref db)))

    ;; test working on couple of examples
    (select (where :rating 8))
    (select (where :title "Fly"))

QED.

### Updating the database

In this section, update is implemented as

1.  map over the current value of db, updating each record if the predicate where clause matches
2.  reset the value of db by the list returned by 1.

For clojure implementation, we do some simplifications. The keys to be updated are taken in as a map (update-map in the code below) rather than keyword arguments. This allows a one-step update of the document. Contrast this where pcl books implementation writes one clause each for each key. The code with apply - assoc - update-seq looks little mysterious and deserves explanation.

I wanted to avoid writing one clause for each key to be updated passed in. One, concise is better. Two, clojure’s maps are immutable :-P, each update of the map would return another updated map. So although the setf on the row works out in the book, clojure code would have needed something ugly. On the other hand, clojure’s assoc function (used for updating maps) can accept multiple keys and values. There’s our clue to simpler implementation. In let binding of update-seq, we convert the map into a list of key-value pairs. In the apply assoc part, we update all the passed in keys at once. Bonus, we don’t have to check for which keys are passed in. Neat!

    (def tmp (flatten (seq {:rating 11 :artist "Guns N Roses"})))
    (apply assoc {:rating 10 :artist "Guns N Poses"} tmp)

    ;; for updating, you move over the list, update it and then reset the value of db

    (defn updatedb
      [select-fn update-map]
      (let [update-seq (flatten (seq update-map))]
        (reset! db
          (map
           #(if (select-fn %)
              (apply assoc % update-seq)
              %) (deref db)))))

    ;; update ratings for the artist we love in one go
    (updatedb (where :artist "Dixie Chicks") {:rating 12})

### Learnings so far

1.  We have a rudimentary document database with select where and update where running in about 38 lines of code.
2.  Common lisp and Clojure are fundamentally different languages. I’ll have to work around Clojure’s immutable by default symantics while studying rest of the book.
3.  I prefer clojures map and filter, functions over sequence abstraction. CL documentation has tons of things like mapc, mapcar, maplist and I’ll need some time to figure out which is which.
4.  Immutability is a strength of clojure. For example, in the CL code given in the book, the plist is mutable (setf/getf possible) whereas mapcar rightly returns another list. It is better to get rid of the mental overhead of bookeeping what is mutable and what is not.
5.  Lisps get things like keyword arguments right, probably since ancient times. I can’t imagine working without them.

In next part, Peter moves on to getting rid of code duplication and making the code more generic (get rid of the use case specific code like assuming artist, rating are the only keys in our document). This part will involve macros and promises to be exciting ride.
