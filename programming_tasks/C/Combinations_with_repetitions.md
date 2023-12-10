[1]: https://rosettacode.org/wiki/Combinations_with_repetitions

# [Combinations with repetitions][1]





One could simply generate all [permutations](https://rosettacode.org/wiki/Permutations_with_repetitions#Raku), and then remove "duplicates":

```perl
my @S = <iced jam plain>;
my $k = 2;

.put for [X](@S xx $k).unique(as => *.sort.cache, with => &[eqv])
```

#### Output:
```
iced iced
iced jam
iced plain
jam jam
jam plain
plain plain
```


Alternatively, a recursive solution:

```perl
proto combs_with_rep (UInt, @) {*}
 
multi combs_with_rep (0,  @)  { () }
multi combs_with_rep (1,  @a)  { map { $_, }, @a }
multi combs_with_rep ($,  []) { () }
multi combs_with_rep ($n, [$head, *@tail]) {
    |combs_with_rep($n - 1, ($head, |@tail)).map({ $head, |@_ }),
    |combs_with_rep($n, @tail);
}
 
.say for combs_with_rep( 2, [< iced jam plain >] );
 
# Extra credit:
sub postfix:<!> { [*] 1..$^n }
sub combs_with_rep_count ($k, $n) { ($n + $k - 1)! / $k! / ($n - 1)! }
 
say combs_with_rep_count( 3, 10 );
```

#### Output:
```
(iced iced)
(iced jam)
(iced plain)
(jam jam)
(jam plain)
(plain plain)
220
```
