[1]: https://rosettacode.org/wiki/Continued_fraction

# [Continued fraction][1]

```perl
sub continued-fraction(:@a, :@b, Int :$n = 100)
{
    my $x = @a[$n - 1];
    $x = @a[$_ - 1] + @b[$_] / $x for reverse 1 ..^ $n;
    $x;
}
 
printf "√2 ≈%.9f\n", continued-fraction(:a(1, |(2 xx *)), :b(Nil, |(1 xx *)));
printf "e  ≈%.9f\n", continued-fraction(:a(2, |(1 .. *)), :b(Nil, 1, |(1 .. *)));
printf "π  ≈%.9f\n", continued-fraction(:a(3, |(6 xx *)), :b(Nil, |((1, 3, 5 ... *) X** 2)));
```

#### Output:
```
√2 ≈ 1.414213562
e  ≈ 2.718281828
π  ≈ 3.141592654
```


A more original and a bit more abstract method would consist in viewing a continued fraction on rank n as a function of a variable x:



Or, more consistently:



Viewed as such, ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=e48ee36b1a3c54ddb341c72e565131ed&mode=mathml) could be written recursively:



Or in other words:



where ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=a59358d8eac1e8f71974271c5d11caf7&mode=mathml)



Perl6 has a builtin composition operator. We can use it with the triangular reduction metaoperator, and evaluate each resulting function at infinity (any value would do actually, but infinite makes it consistent with this particular task).

```perl
sub continued-fraction(@a, @b) {
    map { .(Inf) }, [\o] map { @a[$_] + @b[$_] / * }, ^Inf
}
 
printf "√2 ≈ %.9f\n", continued-fraction((1, |(2 xx *)), (1 xx *))[10];
printf "e  ≈ %.9f\n", continued-fraction((2, |(1 .. *)), (1, |(1 .. *)))[10];
printf "π  ≈ %.9f\n", continued-fraction((3, |(6 xx *)), ((1, 3, 5 ... *) X** 2))[100];
```

#### Output:
```
√2 ≈ 1.414213552
e  ≈ 2.718281827
π  ≈ 3.141592411
```