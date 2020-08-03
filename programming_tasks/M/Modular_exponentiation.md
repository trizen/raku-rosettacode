[1]: https://rosettacode.org/wiki/Modular_exponentiation

# [Modular exponentiation][1]

This is specced as a built-in, but here's an explicit version:

```perl
sub expmod(Int $a is copy, Int $b is copy, $n) {
    my $c = 1;
    repeat while $b div= 2 {
        ($c *= $a) %= $n if $b % 2;
        ($a *= $a) %= $n;
    }
    $c;
}
 
say expmod
    2988348162058574136915891421498819466320163312926952423791023078876139,
    2351399303373464486466122544523690094744975233415544072992656881240319,
    10**40;
```

#### Output:
```
1527229998585248450016808958343740453059
```