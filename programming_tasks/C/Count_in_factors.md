[1]: http://rosettacode.org/wiki/Count_in_factors

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
    while $remainder %% $factor {
        take $factor;
        last if ($remainder div= $factor) === 1;
    }
  }
}
 
say "$_: ", factors($_).join(" × ") for 1..*;
```


The first twenty numbers:


#### Output:
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


Here we use a <tt>multi</tt> declaration with a constant parameter to match the degenerate case. We use <tt>copy</tt> parameters when we wish to reuse the formal parameter as a mutable variable within the function. (Parameters default to readonly in Perl&#160;6.) Note the use of <tt>gather</tt>/<tt>take</tt> as the final statement in the function, which is a common Perl&#160;6 idiom to set up a coroutine within a function to return a lazy list on demand.



Note also the '×' above is not ASCII 'x', but U+00D7 MULTIPLICATION SIGN. Perl&#160;6 does Unicode natively.



Here is a solution inspired from [Almost\_prime#C](/wiki/Almost\_prime#C" title="Almost prime). It doesn't use &amp;is-prime.

```perl
sub factor($n is copy) {
    $n == 1 ?? 1 !!
    gather {
	$n /= take 2 while $n %% 2;
	$n /= take 3 while $n %% 3;
	loop (my $p = 5; $p*$p <= $n; $p+=2) {
	    $n /= take $p while $n %% $p;
	}
	take $n unless $n == 1;
    }
}
 
say "$_ == ", join " \x00d7 ", factor $_ for 1 .. 20;
 
```