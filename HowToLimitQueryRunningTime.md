# Limiting the Query Running Time #

## Problem ##

You want to set a time limit for a query (_e.g., you don't have infinite time ;)_)

## Solution ##

SWI-Prolog has a `call_with_time_limit` predicate
which could be used to terminate the search after sometime; note that
the partial solution list cannot be recovered (as far as I see).
`call_with_time_limit` works on both unix and win32. You need to
import `time` module (_of SWI-Prolog_) to use it. Here's a modified sudoku example with
time limit. Save this in the same directory with pyswip's `sudoku.pl`
(_in `examples/sudoku`_):

```
from pyswip import Prolog 
from pyswip.prolog import PrologError 

_ = 0 
puzzle2 = [ 
            [_,_,1,_,8,_,6,_,4], 
            [_,3,7,6,_,_,_,_,_], 
            [5,_,_,_,_,_,_,_,_], 
            [_,_,_,_,_,5,_,_,_], 
            [_,_,6,_,1,_,8,_,_], 
            [_,_,_,4,_,_,_,_,_], 
            [_,_,_,_,_,_,_,_,3], 
            [_,_,_,_,_,7,5,2,_], 
            [8,_,2,_,9,_,7,_,_] 
          ] 

def solve(problem): 
    prolog = Prolog() 
    list(prolog.query("use_module(library(time))"))  # <-- import time.
    prolog.consult("sudoku.pl") 
    p = str(problem).replace("0", "_") 

    try: 
        result = list(prolog.query("L=%s,call_with_time_limit(5, sudoku(L))" % p))  # <-- limit the query running time.
        if result:  # <-- solution found in time limit
            return result[0]["L"] 
    except PrologError:  # <-- time out detected
            print "Timeout!" 

    return False 

def main(): 
    puzzle = puzzle2 
    print "Search started..." 
    solution = solve(puzzle) 
    if solution: 
        print solution 
    else: 
        print "This puzzle has no solutions [is it valid?]" 

if __name__ == "__main__": 
    main() 
```

`call_with_time_limit` takes the limit in seconds and a goal to prove.
The manual page for it is at: http://www.swi-prolog.org/packages/clib.html#sec:10

That code causes a _timeout_ on my computer for times less than 11
secs.