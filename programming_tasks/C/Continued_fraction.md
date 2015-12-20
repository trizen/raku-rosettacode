[1]: http://rosettacode.org/wiki/Continued_fraction

# [Continued fraction][1]

```perl6
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



Viewed as such, <span class="texhtml" dir="ltr">CF<sub>_n_</sub>(_x_)</span> could be written recursively:



Or in other words:



where <img class="tex" alt="f\_n(x) = a\_n + \frac{b\_n}{x}" src="/mw/images/math/4/7/1/47199469d68ddcdfeb4096f415571576.png"/>



Perl6 has a builtin composition operator. We can use it with the triangular reduction metaoperator, and evaluate each resulting function at infinity (any value would do actually, but infinite makes it consistent with this particular task).

```perl
sub continued-fraction(@a, @b) {
    [map](http://perldoc.perl.org/functions/map.html) { .(Inf) }, [\o] [map](http://perldoc.perl.org/functions/map.html) { @a[$_] + @b[$_] / * }, ^Inf
}
 
[printf](http://perldoc.perl.org/functions/printf.html) "√2 ≈ %.9f\n", continued-fraction((1, |(2 xx *)), (1 xx *))[10];
[printf](http://perldoc.perl.org/functions/printf.html) "e  ≈ %.9f\n", continued-fraction((2, |(1 .. *)), (1, |(1 .. *)))[10];
[printf](http://perldoc.perl.org/functions/printf.html) "π  ≈ %.9f\n", continued-fraction((3, |(6 xx *)), ((1, 3, 5 ... *) X** 2))[100];
```

#### Output:
```
√2 ≈ 1.414213552
e  ≈ 2.718281827
π  ≈ 3.141592411
```