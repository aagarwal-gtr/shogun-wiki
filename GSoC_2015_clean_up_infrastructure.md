# Shogun detox

### Mentors
 * [Thoralf](Thoralf%20Klein) (github: [tklein](https://github.com/tklein23))
 * [Sergey](Sergey%20Lisitsyn) (github: [lisitsyn](https://github.com/lisitsyn), IRC: lisitsyn)
 * [Heiko](Heiko%20Strathmann) (github: [karlnapf](https://github.com/karlnapf), IRC: HeikoS)
 * [Soumyajit](Soumyajit%20De%20[Rahul]) (github: [lambday](https://github.com/lambday), IRC: lambday)

### Difficulty & Requirements
Medium, but requires a lot of initiative and willingness to dive into existing code (that is not pretty)

You need know
 * Shogun's internals (to an extend)
 * C++
 * Software engineering principles
 * No Machine Learning

### Description
Every line of code in SHOGUN has a long history and have gone through
many brains and hands.  This made SHOGUN what it is today: a powerful
toolbox with a lot of features.  But most of the code has been written
by researchers for their studies.  Usually the focus is on "getting
things done", proving awesome ideas and optimize them "as fast as
possible".

The drawback is that people don't care about the software engineering
aspects.  In addition to this, lot of new technologies have shown up
since some parts of the code has been written, which allows us to do
even cooler things with less code.

We want this project to improve maintainability, stablility, and
beauty.  Making common used base classes more lightweight to improve
performance and memory consumption.  Use new and cool technologies,
new language features (think of C++1x) amongst others may be what
this project is about.

### Target students
The target group of this project are people with C/C++ background,
an idea about "good software" engineering, and reliable software.  In
return we offer that you'll learn a lot about basic machine learning
algorithms; of course there are some low-hanging fruits, but if you're
an advanced hacker, we have a lot of great ideas we could push the
project forward.

GSoC is a marathon, not a sprint.  We expect "good" performance over
the whole project and to stay in contact with us.  Get on board and
commit to contribute actively and we'll promise to bring you on speed
with magic internals that are hidden in SHOGUN. :)

### Details
Here are some sub-projects. We are open for more:

* File readers.
  tl;dr: File IO and parsing done right using modern C++.

  SHOGUN contains tons (how many lines?) of code to just parse input
  data formats.  The code is basically working, some of parsers have
  minor bugs, most of them read like "C89 with classes", and static
  code analysis tells us we need to do something here.

  Lot things possible here: refactorings, deduplication, new API, make
  it less code, make it less NIH.

* Redesign data classes.
  tl;dr: Being a software architect.

  The foundation of every learning problem is data structures to be
  used by all algorithms.  Dense/Sparse Features, for instance, or
  Dense/Sparse Streaming... duplicated functionality, special handling
  of feature classes in algorithm code; online algorithms not possible
  on non-stream features.

  Buzzword bingo:  Separation of concerns; finding invariants in
  the existing classes; redesign of features APIs; going back to the
  board and analyze what's really needed; gain flexibility.

* Serialization framework.
  tl;dr: Dirty work with binary data.  Beat the NIH out of here!
   * Working title: Dirty deeds done with with binary data.
   * Alternative working title: Beat the NIH out of here!

  SHOGUN brings its own Serialization framework, which provides a lot
  of functionality and brings *a lot of code*.  Deep-copy of objects?
  Checking equality?  Dump objects to disk and get 'em back?  All done
  in here.  Thousands lines of code, uncountable many switch-case
  statements, and more special per-class and per-data-type code than we
  want to maintain.  Only one good reason why we didn't tackle it yet:
  It's working.

### Waypoints and initial work
  What's to be done here depends on you.  First of all we need to
  analyze and evaluate existing serialization frameworks.  Check if
  we can use protobuf, Boost serialization, etc.  The minimal goal is
  a small prototype to prove the idea.  The full-fletched solution
  is, well, you guessed it: Hard work and lot of fame.

### Optional
Whatever you can imagine

### Why this is cool
It attempts to improve one of the biggest open problems we have in Shogun: Being unable to move because of being chained the framework. A modern, slim Shogun is the dream of every of our developers :)

### Useful ressources
 * All core developers
 * https://github.com/shogun-toolbox/shogun/issues/2113

Data structures:
 * CSGObject
 * Parameter.cpp
 * TParameter
 * all FileReader instances
