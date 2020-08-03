[1]: https://rosettacode.org/wiki/First-class_functions/Use_numbers_analogously

# [First-class functions/Use numbers analogously][1]

```perl
sub multiplied ($g, $f) { return { $g * $f * $^x } }
 
my $x  = 2.0;
my $xi = 0.5;
my $y  = 4.0;
my $yi = 0.25;
my $z  = $x + $y;
my $zi = 1.0 / ( $x + $y );
 
my @numbers = $x, $y, $z;
my @inverses = $xi, $yi, $zi;
 
for flat @numbers Z @inverses { say multiplied($^g, $^f)(.5) }
```


Output:


#### Output:
```
0.5
0.5
0.5
```


The structure of this is identical to first-class function task.