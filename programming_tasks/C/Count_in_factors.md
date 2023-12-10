[1]: https://rosettacode.org/wiki/Count_in_factors

# [Count in factors][1]



```perl
constant @primes = 2, |(3, 5, 7 ... *).grep: *.is-prime;

multi factors(1) { 1 }
multi factors(Int $remainder is copy) {
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

say "$_: ", factors($_).join(" × ") for 1..*;
```


The first twenty numbers:


```
1: 1
2: 2
3: 3
4: 2 × 2
5: 5
6: 2 × 3
7: 7
8: 2 × 2 × 2
9: 3 × 3
10: 2 × 5
11: 11
12: 2 × 2 × 3
13: 13
14: 2 × 7
15: 3 × 5
16: 2 × 2 × 2 × 2
17: 17
18: 2 × 3 × 3
19: 19
20: 2 × 2 × 5
```


Here we use a `multi` declaration with a constant parameter to match the degenerate case.  We use `copy` parameters when we wish to reuse the formal parameter as a mutable variable within the function. (Parameters default to readonly in Raku.) Note the use of `gather`/`take` as the final statement in the function, which is a common Raku idiom to set up a coroutine within a function to return a lazy list on demand.



Note also the '×' above is not ASCII 'x', but U+00D7 MULTIPLICATION SIGN.  Raku does Unicode natively.



Here is a solution inspired from [Almost_prime#C](https://rosettacode.org/wiki/Almost_prime#C).  It doesn't use &amp;is-prime.

```perl
sub factor($n is copy) {
    $n == 1 ?? 1 !!
    gather {
	$n /= take 2 while $n %% 2;
	$n /= take 3 while $n %% 3;
	loop (my $p = 5; $p*$p <= $n; $p+=2) {
	    $n /= take $p while $n %% $p;
	}
	take $n unless $n == 1;
    }
}

say "$_ == ", join " \x00d7 ", factor $_ for 1 .. 20;
```


Same output as above.





Alternately, use a module:

```perl
use Prime::Factor;

say "$_ = {(.&prime-factors || 1).join: ' x ' }" for flat 1 .. 10, 10**20 .. 10**20 + 10;
```

#### Output:
```
1 = 1
2 = 2
3 = 3
4 = 2 x 2
5 = 5
6 = 2 x 3
7 = 7
8 = 2 x 2 x 2
9 = 3 x 3
10 = 2 x 5
100000000000000000000 = 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 2 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5 x 5
100000000000000000001 = 73 x 137 x 1676321 x 5964848081
100000000000000000002 = 2 x 3 x 155977777 x 106852828571
100000000000000000003 = 373 x 155773 x 1721071782307
100000000000000000004 = 2 x 2 x 13 x 1597 x 240841 x 4999900001
100000000000000000005 = 3 x 5 x 7 x 7 x 83 x 1663 x 985694468327
100000000000000000006 = 2 x 31 x 6079 x 265323774602147
100000000000000000007 = 67 x 166909 x 8942221889969
100000000000000000008 = 2 x 2 x 2 x 3 x 3 x 3 x 233 x 1986965506278811
100000000000000000009 = 557 x 72937 x 2461483384901
100000000000000000010 = 2 x 5 x 11 x 909090909090909091
```
