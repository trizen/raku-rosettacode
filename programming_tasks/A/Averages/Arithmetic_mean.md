[1]: http://rosettacode.org/wiki/Averages/Arithmetic_mean

# [Averages/Arithmetic mean][1]

```perl6
multi mean([]){ Failure.new('mean on empty list is not defined') }; # Failure-objects are lazy exceptions
multi mean (@a) { ([+] @a) / @a }
```