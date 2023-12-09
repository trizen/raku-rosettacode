[1]: https://rosettacode.org/wiki/N-grams

# [N-grams][1]

```perl
sub n-gram ($this, $N=2) { Bag.new( flat $this.uc.map: { .comb.rotor($N => -($N-1))».join } ) }
dd 'Live and let live'.&n-gram; # bi-gram
dd 'Live and let live'.&n-gram(3); # tri-gram
```

#### Output:
```
("IV"=>2,"T "=>1,"VE"=>2,"E "=>1,"LE"=>1,"AN"=>1,"LI"=>2,"ND"=>1,"ET"=>1," L"=>2," A"=>1,"D "=>1).Bag
("ET "=>1,"AND"=>1,"LIV"=>2," LI"=>1,"ND "=>1," LE"=>1,"IVE"=>2,"E A"=>1,"VE "=>1,"T L"=>1,"D L"=>1,"LET"=>1," AN"=>1).Bag
```
