[1]: https://rosettacode.org/wiki/Fibonacci_n-step_number_sequences

# [Fibonacci n-step number sequences][1]

### Lazy List with Closure

```raku
use MONKEY-SEE-NO-EVAL;
 
sub fibo ($n) {
    constant @starters = 1,1,2,4 ... *;
    nacci @starters[^$n];
}
 
sub nacci (*@starter) {
    EVAL "|@starter, { join '+', '*' xx @starter } ... *";
}
 
for 2..10 -> $n { say fibo($n)[^20] }
say nacci(2,1)[^20];
```

#### Output:
```
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765
1 1 2 4 7 13 24 44 81 149 274 504 927 1705 3136 5768 10609 19513 35890 66012
1 1 2 4 8 15 29 56 108 208 401 773 1490 2872 5536 10671 20569 39648 76424 147312
1 1 2 4 8 16 31 61 120 236 464 912 1793 3525 6930 13624 26784 52656 103519 203513
1 1 2 4 8 16 32 63 125 248 492 976 1936 3840 7617 15109 29970 59448 117920 233904
1 1 2 4 8 16 32 64 127 253 504 1004 2000 3984 7936 15808 31489 62725 124946 248888
1 1 2 4 8 16 32 64 128 255 509 1016 2028 4048 8080 16128 32192 64256 128257 256005
1 1 2 4 8 16 32 64 128 256 511 1021 2040 4076 8144 16272 32512 64960 129792 259328
1 1 2 4 8 16 32 64 128 256 512 1023 2045 4088 8172 16336 32656 65280 130496 260864
2 1 3 4 7 11 18 29 47 76 123 199 322 521 843 1364 2207 3571 5778 9349
```


### Generative



A slightly more straight forward way of constructing a lazy list.

```raku
sub fib ($n, @xs is copy = [1]) {
    flat gather {
        take @xs[*];
        loop {
            take my $x = [+] @xs;
            @xs.push: $x;
            @xs.shift if @xs > $n;
        }
    }
}
 
for 2..10 -> $n {
    say fib($n, [1])[^20];
}
say fib(2, [2,1])[^20];
```