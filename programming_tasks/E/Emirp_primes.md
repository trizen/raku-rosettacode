[1]: https://rosettacode.org/wiki/Emirp_primes

# [Emirp primes][1]

Build a lazy list using Perl 6's builtin &amp;is-prime, then display results based on parameters passed in.The default is to display an array slice starting and stopping at the given indicies. Alternately, ask for all values between two endpoints.

```perl
sub MAIN ($start, $stop = Nil, $display = <slice>) {
    my $end = $stop // $start; 
    my @emirps = lazy gather for 1 .. * -> $n {
        take $n if $n.is-prime 
          and (+$n.flip).is-prime
          and $n != +($n.flip)
    }
 
    given $display {
        when 'slice'  { say @emirps[$start-1 .. $end-1] }; 
        when 'values' { 
            my @values = gather for @emirps {
                .take if $start < $_ < $end;
                last if $_> $end 
            }
            say @values
        }
    } 
}
```


Run with passed parameters: 1 20



('slice' is the default. you *could* pass it in, but it isn't necessary.)


#### Output:
```
13 17 31 37 71 73 79 97 107 113 149 157 167 179 199 311 337 347 359 389
```


Run with passed parameters: 7700 8000 values


#### Output:
```
7717 7757 7817 7841 7867 7879 7901 7927 7949 7951 7963
```


Run with passed parameter: 10000


#### Output:
```
948349
```