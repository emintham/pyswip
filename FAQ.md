# Introduction #

This document contains questions and answers gathered mainly from PySWIP mailing list. You're welcome to add your questions to this wiki.

# Common Usage #

**1. Q:** Can I limit the number of generated solutions?

**A:** Yes, see [HowToLimitNumberOfFoundSolutions](HowToLimitNumberOfFoundSolutions.md).

**2. Q:** Can I limit the running time of a query?

**A:** Yes, see [HowToLimitQueryRunningTime](HowToLimitQueryRunningTime.md).

**3. Q:** How can I run multiple queries with distinct data?

**A:** See [HowToSimulateMultipleKnowledgebases](HowToSimulateMultipleKnowledgebases.md).

# Errors and Bugs #

**1. Q:** When I import PySWIP, I get:
```
  libpl (shared) not found. Possible reasons:
  1) SWI-Prolog not installed as a shared library. Install SWI-Prolog (5.6.34 works just fine)
```

**A:** If you're on UNIX/Linux, you didn't install SWI-Prolog's shared library (which is **NOT** the default when you compile the sources yourself, and not installed by default for many distributions _such as Debian/Ubuntu_. Most probably you'll have to compile SWI-Prolog on your own; see [INSTALL](INSTALL.md).

**A:** If you're on Windows, you need to set your [path](http://en.wikipedia.org/wiki/Path_(computing)#PATH_variable) so that `libpl.dll` is found; see [INSTALL](INSTALL.md).

**2. Q:** I get the following when  try to run a trivial PySWIP example:
```
  Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "pyswip/prolog.py", line 81, in __call__
    v.update(r)
  ValueError: dictionary update sequence element #0 has length 1; 2 is required
```

**A:** Upgrade your PySWIP to at least version 0.2.2.

**3. Q:** I can't even run the sample programs, since PySWIP segfaults!

**A:** PySWIP still has some problems with 64 bit systems. Please open an issue and post an example code that we'll try to look at that!