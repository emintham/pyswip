# PySWIP Examples #

_(updated for version 0.2)_

## SEND MORE MONEY ##

Finding valid and distinct integers for digits S, E, N, D, M, O, R and Y in the equation SEND + MORE = MONEY is a classical constraint programming problem.

Here's the slightly modified version of the sample program at [Wikipedia constraint programming entry](http://en.wikipedia.org/wiki/Constraint_programming) for SWI-Prolog using clp library:
```
:- use_module(library('clp/bounds')).

sendmore(Digits) :-
   Digits = [S,E,N,D,M,O,R,Y],     % Create variables
   Digits in 0..9,               % Associate domains to variables
   S #\= 0,                        % Constraint: S must be different from 0
   M #\= 0,
   all_different(Digits),           % all the elements must take different values
                1000*S + 100*E + 10*N + D     % Other constraints
              + 1000*M + 100*O + 10*R + E
   #= 10000*M + 1000*O + 100*N + 10*E + Y,
   label(Digits).               % Start the search
```

Save this file as `money.pl`. Here's the Python driver code:
```
from pyswip import Prolog

letters = "S E N D M O R Y".split()
prolog = Prolog()
prolog.consult('money.pl')
for result in prolog.query("sendmore(X)"):
    r = result["X"]
    for i, letter in enumerate(letters):
        print letter, "=", r[i]

print "That's all..."
```

And the result:
```
S = 9
E = 5
N = 6
D = 7
M = 1
O = 0
R = 8
Y = 2
That's all...
```

## Register A Foreign Function ##

_(updated for version 0.2)_

PySWIP includes a (currently incomplete) interface to SWI-Prolog's foreign language interface. With the use of of ctypes which is already imported, you can access and use this interface.

The following example adds a new prediacte to SWI-Prolog runtime, you can see the original code at: http://gollem.science.uva.nl/SWI-Prolog/Manual/foreigninclude.html#sec:9.6.17
```
from pyswip import Prolog, registerForeign, Atom

def atom_checksum(*a):
    if isinstance(a[0], Atom):
        r = sum(ord(c)&0xFF for c in str(a[0]))
        a[1].value = r&0xFF
        return True
    else:
        return False

p = Prolog()
registerForeign(atom_checksum, arity=2)
print list(p.query("X='Python', atom_checksum(X, Y)"))
```
Outputs: `[{'Y': '130', 'X': 'Python'}]`