[1]: https://rosettacode.org/wiki/Egyptian_fractions

# [Egyptian fractions][1]

```raku
role Egyptian {
    method gist {
	join ' + ',
	    ("[{self.floor}]" if self.abs >= 1),
	    map {"1/$_"}, self.denominators;
    }
    method denominators {
	my ($x, $y) = self.nude;
	$x %= $y;
	my @denom = gather ($x, $y) = -$y % $x, $y * take ($y / $x).ceiling
	    while $x;
    }
}
 
say .nude.join('/'), " = ", $_ but Egyptian for 43/48, 5/121, 2014/59;
 
my @sample = map { $_ => .denominators },
    grep * < 1, 
        map {$_ but Egyptian}, 
            (2 .. 99 X/ 2 .. 99);
 
say .key.nude.join("/"),
    " has max denominator, namely ",
    .value.max
        given max :by(*.value.max), @sample;
 
say .key.nude.join("/"),
    " has max number of denominators, namely ",
    .value.elems
        given max :by(*.value.elems), @sample;
```

#### Output:
```
43/48 = 1/2 + 1/3 + 1/16
5/121 = 1/25 + 1/757 + 1/763309 + 1/873960180913 + 1/1527612795642093418846225
2014/59 = [34] + 1/8 + 1/95 + 1/14947 + 1/670223480
8/97 has max denominator, namely 579504587067542801713103191859918608251030291952195423583529357653899418686342360361798689053273749372615043661810228371898539583862011424993909789665
8/97 has max number of denominators, namely 8
```


Because the harmonic series diverges (albeit very slowly), it is possible to write even improper fractions as a sum of distinct unit fractions. Here is a code to do that:

```raku
role Egyptian {
    method gist { join ' + ', map {"1/$_"}, self.list }
    method list {
	my $sum = 0;
	gather for 2 .. * {
	    last if $sum == self;
	    $sum += 1 / .take unless $sum + 1 / $_ > self;
	}
    }
}
 
say 5/4 but Egyptian;
```

#### Output:
```
1/2 + 1/3 + 1/4 + 1/6
```


The list of terms grows exponentially with the value of the fraction, though.