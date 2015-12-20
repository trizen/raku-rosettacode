[1]: http://rosettacode.org/wiki/Entropy/Narcissist

# [Entropy/Narcissist][1]

```perl
say [log](http://perldoc.perl.org/functions/log.html)(2) R/ [+] [map](http://perldoc.perl.org/functions/map.html) -> \p { p * -[log](http://perldoc.perl.org/functions/log.html) p }, .bag.[values](http://perldoc.perl.org/functions/values.html) >>/>> +$_
    given slurp($*PROGRAM_NAME).comb
```

#### Output:
```
4.89351613053006
```