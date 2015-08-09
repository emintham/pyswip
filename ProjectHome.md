_(Current version: 0.2.3)_


---

**2012-12-29** Version 0.2.3 released. It should work on 64 bit systems now.

**2012-01-15** PySWIP is re-licensed under MIT License.

**2012-01-15** Please note that, PySWIP is not ready for production use. It is not being maintained, and it won't be in the near future. Please fork it if you would like to continue its development.


---


PySWIP is a Python - SWI-Prolog bridge enabling to query SWI-Prolog in your Python programs. It features an (incomplete) SWI-Prolog foreign language interface, a utility class that makes it easy querying with Prolog and also a Pythonic interface.

Since PySWIP uses SWI-Prolog as a shared library and ctypes to access it, it doesn't require compilation to be installed.

## Requirements ##

  * Python 2.3 and higher.
  * ctypes 1.0 and higher.
  * SWI-Prolog 5.6.x and higher (not the development branch).
  * libpl as a shared library.
  * Works on Linux and Win32, should work on all POSIX.

## Example (Using Prolog) ##
```
>>> from pyswip import Prolog
>>> prolog = Prolog()
>>> prolog.assertz("father(michael,john)")
>>> prolog.assertz("father(michael,gina)")
>>> list(prolog.query("father(michael,X)"))
[{'X': 'john'}, {'X': 'gina'}]
>>> for soln in prolog.query("father(X,Y)"):
...     print soln["X"], "is the father of", soln["Y"]
...
michael is the father of john
michael is the father of gina
```
Since version 0.1.3 of PySWIP, it is possible to register a Python function as a Prolog predicate through SWI-Prolog's foreign language interface.

## Example (Foreign Functions) ##
```
from pyswip import Prolog, registerForeign

def hello(t):
    print "Hello,", t
hello.arity = 1

registerForeign(hello)

prolog = Prolog()
prolog.assertz("father(michael,john)")
prolog.assertz("father(michael,gina)")    
list(prolog.query("father(michael,X), hello(X)"))
```
Outputs:
```
Hello, john
Hello, gina
```
Since version 0.2, PySWIP contains a 'Pythonic' interface which allows writing predicates in pure Python.

## Example (Pythonic interface) ##
```
from pyswip import Functor, Variable, Query, call

assertz = Functor("assertz", 1)
father = Functor("father", 2)

call(assertz(father("michael","john")))
call(assertz(father("michael","gina")))

X = Variable()
q = Query(father("michael",X))
while q.nextSolution():
    print "Hello,", X.value
q.closeQuery()
```
Outputs:
```
Hello, john
Hello, gina
```

If you want to contribute to PySWIP's development or just ask questions you can join [Google PySWIP Group](http://groups.google.com/group/pyswip).

PySWIP is being developed by Yuce Tekol, you can email me at: `yucetekol [AT] gmail [DOT] com`. Please see [Authors](http://code.google.com/p/pyswip/wiki/Authors) for a full list of contributors.
