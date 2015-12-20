[1]: http://rosettacode.org/wiki/Ackermann_function

# [Ackermann function][1]

```perl6
sub A(Int $m, Int $n) {
    if    $m == 0 { $n + 1 } 
    elsif $n == 0 { A($m - 1, 1) }
    else          { A($m - 1, A($m, $n - 1)) }
}
```


An implementation using multiple dispatch:

```perl6
multi sub A(0,      Int $n) { $n + 1                   }
multi sub A(Int $m, 0     ) { A($m - 1, 1)             }
multi sub A(Int $m, Int $n) { A($m - 1, A($m, $n - 1)) }
```


Note that in either case, Int is defined to be arbitrary precision in Perl&#160;6.



Here's a caching version of that, written in the sigilless style, with liberal use of Unicode, and the extra optimizing terms to make A(4,2) possible:

```perl6
proto A(Int \𝑚, Int \𝑛) { (state @)[𝑚][𝑛] //= {*} }
 
multi A(0,      Int \𝑛) { 𝑛 + 1 }
multi A(1,      Int \𝑛) { 𝑛 + 2 }
multi A(2,      Int \𝑛) { 3 + 2 * 𝑛 }
multi A(3,      Int \𝑛) { 5 + 8 * (2 ** 𝑛 - 1) }
 
multi A(Int \𝑚, 0     ) { A(𝑚 - 1, 1) }
multi A(Int \𝑚, Int \𝑛) { A(𝑚 - 1, A(𝑚, 𝑛 - 1)) }
```


Testing:

```perl6
say A(4,1);
say .chars, " digits starting with ", .substr(0,50), "..." given A(4,2);
```

#### Output:
```
65533
19729 digits starting with 20035299304068464649790723515602557504478254755697...
```