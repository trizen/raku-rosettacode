[1]: http://rosettacode.org/wiki/Narcissistic_decimal_number

# [Narcissistic decimal number][1]

Here is a straightforward, naive implementation. It works but takes ages.

```perl
sub is-narcissistic(Int $n) { $n == [+] $n.comb »**» $n.chars }
 
for 0 .. * {
    if .&is-narcissistic {
	.say;
	last if ++state$ >= 25;
    }
}
```

#### Output:
```
0
1
2
3
4
5
6
7
8
9
153
370
371
407
Ctrl-C
```


Here the program was interrupted but if you're patient enough you'll see all the 25 numbers.



Here's a faster version that precalculates the values for base 1000 digits:

```perl
sub kigits($n) {
    my int $i = $n;
    my int $b = 1000;
    gather while $i {
        take $i % $b;
        $i = $i div $b;
    }
}
 
constant narcissistic = 0, (1..*).map: -> $d {
    my @t = 0..9 X** $d;
    my @table = @t X+ @t X+ @t;
    sub is-narcissistic(\n) { n == [+] @table[kigits(n)] }
    gather take $_ if is-narcissistic($_) for 10**($d-1) ..^ 10**$d;
}
 
for narcissistic {
    say ++state $n, "\t", $_;
    last if $n == 25;
}
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