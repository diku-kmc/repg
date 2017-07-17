# The KMC Kleenex compiler - Parallel Dispatcher #

In its current state the dispatcher can process a target file in parallel by 
dividing the target file into a specified number of chunks. The dispatcher 
currently only work on Kleenex programs that generate no output - (DFA's).

## Build ##

See the general README file for the KMC Kleenex Compiler.

## Use ##

First write the following Kleenex program and save in a file named 'email.kex':

```
emailLines := ( ~simpleEmail ~/\n/ | ~fb ~/\n/ )+

fb := /[^\n]*/

simpleEmail := /[a-z0-9!#$%&'*+\/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+\/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/
```

Next compile a transducer using the `kexc` executable:

```
> kexc compile email.kex --srcout kleenex.c --act=false --la=false --hf=true
```

Next compile the dispatcher:
```
> gcc -O3 -pthread -o dispatcher dispatcher.c kleenex.c thr_pool.c -I.
```

Finally run the dispatcher:
```
> ./dispatcher -f in-file -c chunks
```
Where __in-file__ is the target file, and __chunks__ is the number of chunks to divide the target file into.