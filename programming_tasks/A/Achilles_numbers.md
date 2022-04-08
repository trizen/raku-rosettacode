[1]: https://rosettacode.org/wiki/Achilles_numbers

# [Achilles numbers][1]

Timing is going to be system / OS dependent.

```perl
use Prime::Factor;
use Math::Root;
 
sub is-square-free (Int \n) {
    constant @p = ^100 .map: { next unless .is-prime; .² };
    for @p -> \p { return False if n %% p }
    True
}
 
sub powerful (\n, \k = 2) {
    my @p;
    p(1, 2*k - 1);
    sub p (\m, \r) {
        @p.push(m) and return if r < k;
        for 1 .. (n / m).&root(r) -> \v {
            if r > k {
                next unless is-square-free(v);
                next unless m gcd v == 1;
            }
            p(m * v ** r, r - 1)
        }
    }
    @p
}
 
my $achilles = powerful(10**9).hyper(:500batch).grep( {
    my $f = .&prime-factors.Bag;
    (+$f.keys > 1) && (1 == [gcd] $f.values) && (.sqrt.Int² !== $_)
} ).classify: { .chars }
 
my \𝜑 = 0, |(1..*).hyper.map: -> \t { t × [×] t.&prime-factors.squish.map: { 1 - 1/$_ } }
 
my %as = Set.new: flat $achilles.values».list;
 
my $strong = lazy (flat $achilles.sort».value».list».sort).grep: { ?%as{𝜑[$_]} };
 
put "First 50 Achilles numbers:";
put (flat $achilles.sort».value».list».sort)[^50].batch(10)».fmt("%4d").join("\n");
 
put "\nFirst 30 strong Achilles numbers:";
put   $strong[^30].batch(10)».fmt("%5d").join("\n");
 
put "\nNumber of Achilles numbers with:";
say "$_ digits: " ~ +$achilles{$_} // 0 for 2..9;
 
printf "\n%.1f total elapsed seconds\n", now - INIT now;
```

#### Output:
```
First 50 Achilles numbers:
  72  108  200  288  392  432  500  648  675  800
 864  968  972 1125 1152 1323 1352 1372 1568 1800
1944 2000 2312 2592 2700 2888 3087 3200 3267 3456
3528 3872 3888 4000 4232 4500 4563 4608 5000 5292
5324 5400 5408 5488 6075 6125 6272 6728 6912 7200

First 30 strong Achilles numbers:
  500   864  1944  2000  2592  3456  5000 10125 10368 12348
12500 16875 19652 19773 30375 31104 32000 33275 37044 40500
49392 50000 52488 55296 61731 64827 67500 69984 78608 80000

Number of Achilles numbers with:
2 digits: 1
3 digits: 12
4 digits: 47
5 digits: 192
6 digits: 664
7 digits: 2242
8 digits: 7395
9 digits: 24008

6.1 total elapsed seconds
```


Could go further but slows to a crawl and starts chewing up memory in short order.


#### Output:
```
10 digits: 77330
11 digits: 247449
12 digits: 788855

410.4 total elapsed seconds
```
