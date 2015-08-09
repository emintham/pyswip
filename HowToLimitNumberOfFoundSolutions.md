# Limiting the Number of Generated Solutions #

## Problem ##

You want to limit the number of generated solutions (_e.g., to decrease the time to execute the query_).

## Solution ##

You have several ways to get only first N solutions, the easiest one
is, passing `maxresult=N` to prolog.query:

```
from pyswip import Prolog 

p = Prolog() 

p.assertz("father(john,mich)") 
p.assertz("father(john,gina)") 
p.assertz("father(john,elvis)") 
p.assertz("father(john,elvira)") 

# Retrieve only 2 solutions 
soln = [s["Y"] for s in p.query("father(john,Y)", maxresult=2)] 
print soln
```

Outputs:
```
['mich', 'gina'] 
```

Since `prolog.query` returns a generator, you can also use the
following:

```
from itertools import imap 
soln = list(imap(lambda i, s:s["Y"], xrange(2), 
p.query("father(john,Y)")))
```