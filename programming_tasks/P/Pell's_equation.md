[1]: https://rosettacode.org/wiki/Pell's_equation

# [Pell's equation][1]

```perl
sub pell (Int $n) {
 
    my $y = my $x = Int(sqrt $n);
    my $z = 1;
    my $r = 2 * $x;
 
    my ($e1, $e2) = (1, 0);
    my ($f1, $f2) = (0, 1);
 
    loop {
        $y = $r * $z - $y;
        $z = Int(($n - $y²) / $z);
        $r = Int(($x + $y)  / $z);
 
        ($e1, $e2) = ($e2, $r * $e2 + $e1);
        ($f1, $f2) = ($f2, $r * $f2 + $f1);
 
        my $A = $e2 + $x * $f2;
        my $B = $f2;
 
        if ($A² - $n * $B² == 1) {
            return ($A, $B);
        }
    }
}
 
for 61, 109, 181, 277 -> $n {
    my ($x, $y) = pell($n);
    printf("x² - %3dy² = 1 for x = %-21s and y = %s\n", $n, $x, $y);
}
```

#### Output:
```
x² -  61y² = 1 for x = 1766319049            and y = 226153980
x² - 109y² = 1 for x = 158070671986249       and y = 15140424455100
x² - 181y² = 1 for x = 2469645423824185801   and y = 183567298683461940
x² - 277y² = 1 for x = 159150073798980475849 and y = 9562401173878027020
```