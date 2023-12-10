[1]: https://rosettacode.org/wiki/Narcissistic_decimal_number

# [Narcissistic decimal number][1]





### Simple, with concurrency



Simple implementation is not exactly speedy, but concurrency helps move things along.

```perl
sub is-narcissistic(Int $n) { $n == [+] $n.comb »**» $n.chars }
my @N = lazy (0..∞).hyper.grep: *.&is-narcissistic;
@N[^25].join(' ').say;
```

#### Output:
```
0 1 2 3 4 5 6 7 8 9 153 370 371 407 1634 8208 9474 54748 92727 93084 548834 1741725 4210818 9800817 9926315
```


### Single-threaded, with precalculations



This version that precalculates the values for base 1000 digits, but despite the extra work ends up taking more wall-clock time than the simpler version.

```perl
sub kigits($n) {
    my int $i = $n;
    my int $b = 1000;
    gather while $i {
        take $i % $b;
        $i = $i div $b;
    }
}

for (1..*) -> $d {
    my @t = 0..9 X** $d;
    my @table = @t X+ @t X+ @t;
    sub is-narcissistic(\n) { n == [+] @table[kigits(n)] };
    state $l = 2;
    FIRST say "1\t0";
    say $l++, "\t", $_ if .&is-narcissistic for 10**($d-1) ..^ 10**$d;
    last if $l > 25
};
```

#### Output:
```
1       0
2       1
3       2
4       3
5       4
6       5
7       6
8       7
9       8
10      9
11      153
12      370
13      371
14      407
15      1634
16      8208
17      9474
18      54748
19      92727
20      93084
21      548834
22      1741725
23      4210818
24      9800817
25      9926315
```
