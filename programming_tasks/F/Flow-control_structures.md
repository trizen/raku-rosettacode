[1]: https://rosettacode.org/wiki/Flow-control_structures

# [Flow-control structures][1]

### Control exceptions



Control flow is extensible in Perl 6; most abnormal control flow (including the standard loop and switch exits) is managed by throwing control exceptions that are caught by the code implementing the construct in question. Warnings are also handled via control exceptions, and turn into control flow if the dynamic context chooses not to resume after the warning. See [[S04/Control exceptions](http://perlcabal.org/syn/S04.html#Control_Exceptions)] for more information.



### Phasers



Phasers are blocks that are transparent to the normal control flow but that are automatically called at an appropriate phase of compilation or execution. The current list of phasers may be found in [[S04/Phasers](http://perlcabal.org/syn/S04.html#Phasers)].



### goto

```perl
TOWN: goto TOWN;
```


Labels that have not been defined yet must be enclosed in quotes.