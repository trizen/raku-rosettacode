[1]: https://rosettacode.org/wiki/Cumulative_standard_deviation

# [Cumulative standard deviation][1]

Using a closure:

```raku
sub sd (@a) {
    my $mean = @a R/ [+] @a;
    sqrt @a R/ [+] map (* - $mean)**2, @a;
}
 
sub sdaccum {
    my @a;
    return { push @a, $^x; sd @a; };
}
 
my &f = sdaccum;
say f $_ for 2, 4, 4, 4, 5, 5, 7, 9;
```


Using a state variable:

```raku
# remember that <(x-<x>)²> = <x²> - <x>²
sub stddev($x) {
    sqrt
        (.[2] += $x**2) / ++.[0] -
        ((.[1] += $x) / .[0])**2
    given state @;
}
 
say stddev $_ for <2 4 4 4 5 5 7 9>;
```

#### Output:
```
0
1
0.942809041582063
0.866025403784439
0.979795897113271
1
1.39970842444753
2
```