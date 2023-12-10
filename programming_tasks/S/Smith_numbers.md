[1]: https://rosettacode.org/wiki/Smith_numbers

# [Smith numbers][1]



```perl
constant @primes = 2, |(3, 5, 7 ... *).grep: *.is-prime;
 
multi factors ( 1 ) { 1 }
multi factors ( Int $remainder is copy ) {
  gather for @primes -> $factor {
 
    # if remainder < factor², we're done
    if $factor * $factor > $remainder {
      take $remainder if $remainder > 1;
      last;
    }
 
    # How many times can we divide by this prime?
    while $remainder %% $factor {
        take $factor;
        last if ($remainder div= $factor) === 1;
    }
  }
}
# Code above here is verbatim from RC:Count_in_factors#Raku

sub is_smith_number ( Int $n ) {
  (!$n.is-prime) and ( [+] $n.comb ) == ( [+] factors($n).join.comb );
}

my @s = grep &is_smith_number, 2 ..^ 10_000;
say "{@s.elems} Smith numbers below 10_000";
say 'First 10: ', @s[  ^10      ];
say 'Last  10: ', @s[ *-10 .. * ];
```

#### Output:
```
376 Smith numbers below 10_000
First 10: (4 22 27 58 85 94 121 166 202 265)
Last  10: (9843 9849 9861 9880 9895 9924 9942 9968 9975 9985)
```
