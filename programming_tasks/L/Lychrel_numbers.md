[1]: http://rosettacode.org/wiki/Lychrel_numbers

# [Lychrel numbers][1]

```perl
my @lychrels;
my @palindromes;
my @seeds;
my $max = 500;
 
for 1 .. 10000 -> $int {
    my @test;
    my $count = 0;
    @lychrels.push( $int => [@test] ) if lychrel($int);
 
    sub lychrel (Int $l) {
        return True if $count++ > $max;
        @test.push: my $m = $l + $l.flip;
        return False if $m == $m.flip;
        lychrel($m);
    }
}
 
@seeds = @lychrels[0];
for @lychrels -> $l {
    @palindromes.push: $l.key if $l.key == $l.key.flip;
    my $trial = 0;
    for @seeds -> $s {
        last if any($s.value) ~~ any($l.value);
        $trial++;
    }
    @seeds.push: $l if $trial == +@seeds;
}
 
say "   Number of Lychrel seed numbers < 10_000: ", +@seeds;
say "             Lychrel seed numbers < 10_000: ", join ", ", @seeds>>.keys;
say "Number of Lychrel related numbers < 10_000: ", +@lychrels -@seeds;
say "    Number of Lychrel palindromes < 10_000: ", +@palindromes;
say "              Lychrel palindromes < 10_000: ", join ", ", @palindromes;
```

#### Output:
```
   Number of Lychrel seed numbers < 10_000: 5
             Lychrel seed numbers < 10_000: 196, 879, 1997, 7059, 9999
Number of Lychrel related numbers < 10_000: 244
    Number of Lychrel palindromes < 10_000: 3
              Lychrel palindromes < 10_000: 4994, 8778, 9999
```