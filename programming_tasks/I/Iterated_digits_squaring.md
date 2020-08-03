[1]: https://rosettacode.org/wiki/Iterated_digits_squaring

# [Iterated digits squaring][1]

This fairly abstract version does caching and filtering to reduce the number of values it needs to check and moves calculations out of the hot loop, but is still interminably slow... even for just up to 1,000,000.

```perl
constant @sq = ^10 X** 2;
my $cnt = 0;
 
sub Euler92($n) {
    $n == any(1,89) ?? $n !!
    (state %){$n} //= Euler92( [+] @sq[$n.comb] )
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


All is not lost, however. Through the use of gradual typing, Perl 6 scales down as well as up, so this jit-friendly version is performant enough to brute force the larger calculation:

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


This runs in under ten minutes. We can reduce this significantly by writing in the NQP (Not Quite Perl6) subset of the language:

```perl
use nqp;
my $cache := nqp::list_i();
nqp::bindpos_i($cache, 650, 0);
nqp::bindpos_i($cache, 1, 1);
nqp::bindpos_i($cache, 89, 89);
 
sub Euler92(int $n) {
    $n < 650
	?? nqp::bindpos_i($cache,$n,ids($n))
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
	    $t = nqp::div_i($n, $ten);
	    $c = $n - $t * $ten;
	    $sum = $sum + $c * $c;
	} while $n = $t;
	$n = nqp::atpos_i($cache,$sum) || $sum;
    }
    $n;
}
 
my int $cnt = 0;
for 1 .. 100_000_000 -> int $n {
   $cnt = $cnt + 1 if Euler92($n) == 89;
}
say $cnt;
```