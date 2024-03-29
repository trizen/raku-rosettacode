[1]: https://rosettacode.org/wiki/Pell%27s_equation

# [Pell&#039;s equation][1]



```perl
use Lingua::EN::Numbers;

sub pell (Int $n) {

    my $y = my $x = Int(sqrt $n);
    my $z = 1;
    my $r = 2 * $x;

    my ($e1, $e2) = (1, 0);
    my ($f1, $f2) = (0, 1);

    loop {
        $y = $r * $z - $y;
        $z = Int(($n - $y²) / $z);
        $r = Int(($x + $y) / $z);

        ($e1, $e2) = ($e2, $r * $e2 + $e1);
        ($f1, $f2) = ($f2, $r * $f2 + $f1);

        my $A = $e2 + $x * $f2;
        my $B = $f2;

        if ($A² - $n * $B² == 1) {
            return ($A, $B);
        }
    }
}

for 61, 109, 181, 277, 8941 -> $n {
    next if $n.sqrt.narrow ~~ Int;
    my ($x, $y) = pell($n);
    printf "x² - %sy² = 1 for:\n\tx = %s\n\ty = %s\n\n", $n, |($x, $y)».&comma;
}
```

#### Output:
```
x² - 61y² = 1 for:
        x = 1,766,319,049
        y = 226,153,980

x² - 109y² = 1 for:
        x = 158,070,671,986,249
        y = 15,140,424,455,100

x² - 181y² = 1 for:
        x = 2,469,645,423,824,185,801
        y = 183,567,298,683,461,940

x² - 277y² = 1 for:
        x = 159,150,073,798,980,475,849
        y = 9,562,401,173,878,027,020

x² - 8941y² = 1 for:
        x = 2,565,007,112,872,132,129,669,406,439,503,954,211,359,492,684,749,762,901,360,167,370,740,763,715,001,557,789,090,674,216,330,243,703,833,040,774,221,628,256,858,633,287,876,949,448,689,668,281,446,637,464,359,482,677,366,420,261,407,112,316,649,010,675,881,349,744,201
        y = 27,126,610,172,119,035,540,864,542,981,075,550,089,190,381,938,849,116,323,732,855,930,990,771,728,447,597,698,969,628,164,719,475,714,805,646,913,222,890,277,024,408,337,458,564,351,161,990,641,948,210,581,361,708,373,955,113,191,451,102,494,265,278,824,127,994,180
```
