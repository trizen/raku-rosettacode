[1]: https://rosettacode.org/wiki/Iterated_digits_squaring

# [Iterated digits squaring][1]


This fairly abstract version does caching and filtering to reduce the number of values it needs to check and moves calculations out of the hot loop, but is still interminably slow... even for just up to 1,000,000.

```perl
constant @sq = ^10 X** 2;
my $cnt = 0;

sub Euler92($n) {
    $n == any(1,89) ?? $n !!
    (state %){$n} //= Euler92( [+] @sq[$n.comb] )
}

for 1 .. 1_000_000 -> $n {
   my $i = +$n.comb.sort.join;
   ++$cnt if Euler92($i) == 89;
}
 
say $cnt;
```

#### Output:
```
856929
```


All is not lost, however. Through the use of gradual typing, Raku scales down as well as up, so this jit-friendly version is performant enough to brute force the larger calculation:

```perl
my @cache;
@cache[1] = 1;
@cache[89] = 89;

sub Euler92(int $n) {
    $n < 649  # 99,999,999 sums to 648, so no point remembering more
        ?? (@cache.AT-POS($n) //= ids($n))
        !! ids($n)
}

sub ids(int $num --> int) {
    my int $n = $num;
    my int $ten = 10;
    my int $sum = 0;
    my int $t;
    my int $c;
    repeat until $n == 89 or $n == 1 {
        $sum = 0;
        repeat {
            $t = $n div $ten;
            $c = $n - $t * $ten;
            $sum = $sum + $c * $c;
        } while $n = $t;
        $n = @cache.AT-POS($sum) // $sum;
    }
    $n;
}

my int $cnt = 0;
for 1 .. 100_000_000 -> int $n {
   $cnt = $cnt + 1 if Euler92($n) == 89;
}
say $cnt;
```

#### Output:
```
85744333
```


Which is better, but is still pretty slow.



The biggest gains are often from choosing the right algorithm.

```perl
sub endsWithOne($n --> Bool) {
    my $digit;
    my $sum = 0;
    my $nn = $n;
    loop {
        while $nn > 0 {
            $digit = $nn % 10;
            $sum += $digit²;
            $nn = $nn div 10;
        }
        return True  if $sum == 1;
        return False if $sum == 89;
        $nn = $sum;
        $sum = 0;
    }
}

my $k = 8; # 10**8
my @sums is default(0) = 1,0;
my $s;
for 1 .. $k -> $n {
    for $n*81 … 1 -> $i {
        for 1 .. 9 -> $j {
            $s = $j²;
            last if $s > $i;
            @sums[$i] += @sums[$i - $s];
        }
    }
}

my $ends-with-one = sum flat @sums[(1 .. $k*81).grep: { endsWithOne($_) }], +endsWithOne(10**$k);

say 10**$k - $ends-with-one;
```

#### Output:
```
85744333
```
