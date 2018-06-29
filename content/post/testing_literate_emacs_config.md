---
title: "Testing Literate Emacs Config"
date: 2016-12-12
tags: ["emacs", "org-mode"]
draft: false
---

Some time back, I wrote my Emacs configuration in an org mode file. The whole setup is documented here. The objective was twofold: one to make it readable to my dumb brain when I open it six months later, second was to make it reproducible on another machine quickly. Say, if something unforseen was to happen to my good ol Dell Vostro machine, what with the configuration safe on Github and all that, I can have my new machine with the Emacs setup very fast and without splitting hair.

Deep inside, I was bit skeptical. Reproducing basic Emacs setup was simple (like inhibiting startup screen, line number mode and other tidbits). The fear was with libraries/packages. I got opportunity to face my fears, so to say, firsthand over weekend.

I got a laptop upgrade (?) at work, with new Thinkpad machine replacing the good old Vostro. I installed Emacs 24.4 from source, copied my Emacs configuration from Git at right place, just added (org-babel-load-file “config-path”) to my init file and bam! The Emacs setup almost like the one on old machine ready within few minutes. Basic setup, theme, cider/clojure setup, emms for music all good. The Python autocomplete piece is not working very cleanly, probably because the original configuration is not correct. But good experience so far.

I know it is supposed to work this way, that’s the whole point. But yeah, after having seen botched configurations and time wastage on new machine setup in the past, I am happy with the result this time around.
