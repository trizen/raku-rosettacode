[1]: https://rosettacode.org/wiki/Taxicab_numbers

# [Taxicab numbers][1]

This uses a pretty simple search algorithm that doesn't necessarily return the Taxicab numbers in order. Assuming we want all the Taxicab numbers within some range S to N, we'll search until we find N values. When we find the Nth value, we continue to search up to the cube root of the largest Taxicab number found up to that point. That ensures we will find all of them inside the desired range without needing to search arbitrarily or use magic numbers. Defaults to returning the Taxicab numbers from 1 to 25. Pass in a different start and end value if you want some other range.

```raku
sub MAIN ($start = 1, $end = 25) {
    my %taxi;
    my $taxis = 0;
    my $terminate = 0;
 
    for 1 .. * -> $c1 {
        display( %taxi, $start, $end ) and exit if 0 < $terminate < $c1;
        my $c = $c1 ** 3;
        for 1 .. $c1 -> $c2 {
            my $this = $c2 ** 3 + $c;
            %taxi{$this}.push: [$c2, $c1];
            $taxis++ if %taxi{$this}.elems == 2;
    	    $terminate = %taxi.grep( { $_.value.elems > 1 } ).sort( +*.key )[*-1].key**(1/3)
                if $taxis == $end and !$terminate;
        }
    }
}
 
sub display (%this_stuff, $start, $end) {
    my $i = $start; 
    printf "%4d %10d  =>\t%s\n", $i++, $_.key, 
        ($_.value.map({ sprintf "%4d³ + %-s", $_[0], "$_[1]\³" })).join: ",\t"
        for %this_stuff.grep( { $_.value.elems > 1 } ).sort( +*.key )[$start-1..$end-1];
    1;
}
```

#### Output:
```
   1       1729  =>        9³ + 10³,       1³ + 12³
   2       4104  =>        9³ + 15³,       2³ + 16³
   3      13832  =>       18³ + 20³,       2³ + 24³
   4      20683  =>       19³ + 24³,      10³ + 27³
   5      32832  =>       18³ + 30³,       4³ + 32³
   6      39312  =>       15³ + 33³,       2³ + 34³
   7      40033  =>       16³ + 33³,       9³ + 34³
   8      46683  =>       27³ + 30³,       3³ + 36³
   9      64232  =>       26³ + 36³,      17³ + 39³
  10      65728  =>       31³ + 33³,      12³ + 40³
  11     110656  =>       36³ + 40³,       4³ + 48³
  12     110808  =>       27³ + 45³,       6³ + 48³
  13     134379  =>       38³ + 43³,      12³ + 51³
  14     149389  =>       29³ + 50³,       8³ + 53³
  15     165464  =>       38³ + 48³,      20³ + 54³
  16     171288  =>       24³ + 54³,      17³ + 55³
  17     195841  =>       22³ + 57³,       9³ + 58³
  18     216027  =>       22³ + 59³,       3³ + 60³
  19     216125  =>       45³ + 50³,       5³ + 60³
  20     262656  =>       36³ + 60³,       8³ + 64³
  21     314496  =>       30³ + 66³,       4³ + 68³
  22     320264  =>       32³ + 66³,      18³ + 68³
  23     327763  =>       51³ + 58³,      30³ + 67³
  24     373464  =>       54³ + 60³,       6³ + 72³
  25     402597  =>       56³ + 61³,      42³ + 69³
```


With passed parameters 2000 2006:


#### Output:
```
2000 1671816384  =>      940³ + 944³,    428³ + 1168³
2001 1672470592  =>      632³ + 1124³,    29³ + 1187³
2002 1673170856  =>      828³ + 1034³,   458³ + 1164³
2003 1675045225  =>      744³ + 1081³,   522³ + 1153³
2004 1675958167  =>      711³ + 1096³,   492³ + 1159³
2005 1676926719  =>      714³ + 1095³,    63³ + 1188³
2006 1677646971  =>      891³ + 990³,     99³ + 1188³
```