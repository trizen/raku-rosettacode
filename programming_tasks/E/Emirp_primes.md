[1]: https://rosettacode.org/wiki/Emirp_primes

# [Emirp primes][1]





For better performance, build the lazy list using module `Math::Primesieve`, not the built-in, then display results based on parameters passed in. The default is to display an array slice starting and stopping at the given indices. Alternately, ask for all values between two endpoints.

```perl
use Math::Primesieve;

sub prime-hash (Int $max) {
    my $sieve = Math::Primesieve.new;
    my @primes = $sieve.primes($max);
    @primes.Set;
}

sub MAIN ($start, $stop = Nil, $display = <slice>) {
    my $end = $stop // $start;
    my %primes = prime-hash(100*$end);
    my @emirps = lazy gather for 1 .. * -> $n {
        take $n if %primes{$n} and %primes{$n.flip} and $n != $n.flip
    }

    given $display {
        when 'slice'  { return @emirps[$start-1 .. $end-1] };
        when 'values' {
            my @values = gather for @emirps {
                .take if $start < $_ < $end;
                last if $_> $end
            }
            return @values
        }
    }
}
```


Run with passed parameters: 1 20



('slice' is the default. you *could* pass it in, but it isn't necessary.)


```
13 17 31 37 71 73 79 97 107 113 149 157 167 179 199 311 337 347 359 389
```


Run with passed parameters: 7700 8000 values


```
7717 7757 7817 7841 7867 7879 7901 7927 7949 7951 7963
```


Run with passed parameter: 10000


```
948349
```
