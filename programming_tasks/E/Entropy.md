[1]: http://rosettacode.org/wiki/Entropy

# [Entropy][1]

```perl
sub entropy(@a) {
    [+] [map](http://perldoc.perl.org/functions/map.html) -> \p { p * -[log](http://perldoc.perl.org/functions/log.html) p }, bag(@a).[values](http://perldoc.perl.org/functions/values.html) »/» +@a;
}
 
say [log](http://perldoc.perl.org/functions/log.html)(2) R/ entropy '1223334444'.comb;
```

#### Output:
```
1.84643934467102
```


In case we would like to add this function to Perl 6's core, here is one way it could be done:

```perl
use MONKEY-TYPING;
augment class Bag {
    method entropy {
	[+] [map](http://perldoc.perl.org/functions/map.html) -> \p { - p * [log](http://perldoc.perl.org/functions/log.html) p },
	self.[values](http://perldoc.perl.org/functions/values.html) »/» +self;
    }
}
 
say '1223334444'.comb.Bag.entropy / [log](http://perldoc.perl.org/functions/log.html) 2;
```