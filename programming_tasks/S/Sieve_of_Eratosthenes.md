[1]: http://rosettacode.org/wiki/Sieve_of_Eratosthenes

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









A recursive version:

```perl
multi erat(Int $N) { erat 2 .. $N }
multi erat(@a where @a[0] > sqrt @a[*-1]) { @a }
multi erat(@a) { @a[0], erat(@a.grep: * % @a[0]) }
 
say erat 100;
```


Of course, upper limits are for wusses. Here's a version using infinite streams, that just keeps going until you ^C it (works under Niecza, but not current Rakudo):

```perl
role Filter[Int $factor] {
    method next { repeat until $.value % $factor { callsame } }
}
 
class Stream {
    has Int $.value is rw = 1;
 
    method next { ++$.value }
    method filter { self but Filter[$.value] }
}
 
.next, say .value for Stream.new, *.filter ... *;
```