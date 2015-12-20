[1]: http://rosettacode.org/wiki/Entropy

# [Entropy][1]

```perl
sub entropy(@a) {
    [+] map -> \p { p * -log p }, bag(@a).values »/» +@a;
}
 
say log(2) R/ entropy '1223334444'.comb;
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
	[+] map -> \p { - p * log p },
	self.values »/» +self;
    }
}
 
say '1223334444'.comb.Bag.entropy / log 2;
```