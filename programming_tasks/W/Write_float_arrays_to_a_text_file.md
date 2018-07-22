[1]: https://rosettacode.org/wiki/Write_float_arrays_to_a_text_file

# [Write float arrays to a text file][1]

```perl
sub writedat ( $filename, @x, @y, $x_precision = 3, $y_precision = 5 ) {
    my $fh = open $filename, :w;
 
    for @x Z @y -> $x, $y {
        $fh.printf: "%.*g\t%.*g\n", $x_precision, $x, $y_precision, $y;
    }
 
    $fh.close;
}
 
my @x = 1, 2, 3, 1e11;
my @y = @x.map({.sqrt});
 
writedat( 'sqrt.dat', @x, @y );
```


File contents


#### Output:
```
1       1
2       1.4142
3       1.7321
1e+11   3.1623e+05
```


In Perl 6 Real::base can be used to convert to Str with arbitrary precision and any base you like. Using the hyper-operator &gt;&gt;. let us strip loops, many temporary variables and is a candidate for autothreading.

```perl
sub writefloat($filename, @x, @y, :$x-precision = 3, :$y-precision = 5) {
    constant TAB = "\t" xx *;
    constant NL = "\n" xx *;
 
    open($filename, :w).print(
        flat @x>>.base(10, $x-precision) Z TAB Z @y>>.base(10, $y-precision) Z NL);
}
my @x = 1, 2, 3, 1e11;
writefloat('sqrt.dat', @x, @x>>.sqrt, :y-precision(20));
```


File contents


#### Output:
```
1.000   1.00000000000000000000
2.000   1.41421356237309510107
3.000   1.73205080756887719318
100000000000.000        316227.76601683790795505047
```