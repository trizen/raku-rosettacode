[1]: https://rosettacode.org/wiki/Write_float_arrays_to_a_text_file

# [Write float arrays to a text file][1]





### Perl 5-ish



Written in the style of the 2nd Perl 5 example.

```perl
sub writefloat ( $filename, @x, @y, $x_precision = 3, $y_precision = 5 ) {
    my $fh = open $filename, :w;
    for flat @x Z @y -> $x, $y {
        $fh.printf: "%.*g\t%.*g\n", $x_precision, $x, $y_precision, $y;
    }
    $fh.close;
}
 
my @x = 1, 2, 3, 1e11;
my @y = @x.map({.sqrt});

writefloat( 'sqrt.dat', @x, @y );
```

#### Output:
```
1       1
2       1.4142
3       1.7321
1e+11   3.1623e+05
```


### Idiomatic



Written in a more idiomatic style.

```perl
sub writefloat($filename, @x, @y, :$x-precision = 3, :$y-precision = 3) {
    open($filename, :w).print:
        join '', flat (@x».fmt("%.{$x-precision}g") X "\t") Z (@y».fmt("%.{$y-precision}g") X "\n");
}
my @x = 1, 2, 3, 1e11;
writefloat('sqrt.dat', @x, @x».sqrt, :y-precision(5));
```

#### Output:
```
1       1
2       1.4142
3       1.7321
1e+11   3.1623e+05
```
