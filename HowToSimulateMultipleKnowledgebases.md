# Simulating Multiple Knowledgebases #

## Problem ##
You want to run two separate queries through Prolog, each with its own data.

## Solution ##

We can have only one prolog knowledgebase for each process as
confirmed in a recent thread (for JPL) (this is a limitation of the
SWI-Prolog foreign language interface):
http://www.nabble.com/JPL-error-t3911134.html

In that thread, Jan Wielemaker also says that, predicates are local to modules,
so we can use modules to simulate many databases.

### Example 1 ###

In this example, two modules named _test_ and _othermod_ is used, please see [SWI-Prolog Dynamic Modules](http://www.lix.polytechnique.fr/~catuscia/teaching/prolog/Manual/sec-4.7.html).
```
from pyswip import Prolog 
p = Prolog() 

# Adding facts and rules to _test_ module:
p.assertz('test:a(z)') 
p.assertz('test:b(z)') 
p.assertz('test:(c(X):-a(X),b(X))')

# Adding facts to _othermod_ module:
p.assertz('othermod:a(z)') 
p.assertz('othermod:b(z)') 

# Querying _test_ module:
print list(p.query('test:c(X)'))
```

Running that exampe outputs: `[{'X': 'z'}]`

### Example 2 ###

Here's  an example to use modules to have multiple knowledgebases. The same example is also present in the `examples` directory. This example uses the Pythonic interface.
```
from pyswip import *

assertz = Functor("assertz")
parent = Functor("parent", 2)
test1 = newModule("test1")
test2 = newModule("test2")

call(assertz(parent("john", "bob")), test1)
call(assertz(parent("jane", "bob")), test1)

call(assertz(parent("mike", "bob")), test2)
call(assertz(parent("gina", "bob")), test2)

print "knowledgebase test1"

X = Variable()
q = Query(parent(X, "bob"), module=test1)
while q.nextSolution():
    print X.value
q.closeQuery()

print "knowledgebase test2"

q = Query(parent(X, "bob"), module=test2)
while q.nextSolution():
    print X.value
q.closeQuery()
```
Outputs:
```
knowledgebase test1
john
jane
knowledgebase test2
mike
gina
```
It shouldn't be hard to write a class that automatically creates new
modules.