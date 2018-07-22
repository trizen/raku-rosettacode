[1]: https://rosettacode.org/wiki/Sieve_of_Eratosthenes

# [Sieve of Eratosthenes][1]

```perl
sub sieve( Int $limit ) {
    my @is-prime = False, False, slip True xx $limit - 1;
 
    gather for @is-prime.kv -> $number, $is-prime {
        if $is-prime {
            take $number;
            loop (my $s = $number**2; $s <= $limit; $s += $number) {
                @is-prime[$s] = False;
            }
        }
    }
}
 
(sieve 100).join(",").say;
```


### A set-based approach



More or less the same as the first Python example:

```perl
sub eratsieve($n) {
    # Requires n(1 - 1/(log(n-1))) storage
    my $multiples = set();
    gather for 2..$n -> $i {
        unless $i (&) $multiples { # is subset
            take $i;
            $multiples (+)= set($i**2, *+$i ... (* > $n)); # union
        }
    }
}
 
say flat eratsieve(100);
```


This gives:


#### Output:
```
 (2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97)
```