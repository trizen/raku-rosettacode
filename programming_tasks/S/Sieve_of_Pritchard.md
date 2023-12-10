[1]: https://rosettacode.org/wiki/Sieve_of_Pritchard

# [Sieve of Pritchard][1]

First, a direct translation of the implementation in the YouTube video:

```perl
unit sub MAIN($limit = 150);

my $maxS = 1;
my $length = 2;
my $p = 3;
my @s = ();

while $p*$p <= $limit {
  if $length < $limit {
    extend-to [$p*$length, $limit].min;
  }
  delete-multiples-of($p);
  $p = next(1);
}
if $length < $limit {
  extend-to $limit;
}

# Done, build the list of actual primes from the array
$p = 3;
my @primes = 2, |gather while $p <= $limit {
  take $p;
  $p = next($p);
};
say @primes;

exit;

sub extend-to($n) {
  my $w = 1;
  my $x = $length + 1;
  while $x <= $n {
     append $x;
     $w = next($w);
     $x = $length + $w;
  }
  $length = $n;
  if $length == $limit {
    append $limit+2;
  }
}

sub delete-multiples-of($p) {
  my $f = $p;
  while $p*$f <= $length {
    $f = next($f);
  }
  while $f > 1 {
    $f = prev($f);
    delete($p*$f);
  }
}

sub append($w) {
  @s[$maxS-1] = $w;
  @s[$w-2] = $maxS;
  $maxS = $w;
}

sub next($w) { @s[$w-1]; }
sub prev($w) { @s[$w-2]; }

sub delete($pf) {
  my $temp1 = @s[$pf-2];
  my $temp2 = @s[$pf-1];
  @s[$temp1-1] = $temp2;
  @s[($temp2-2)%@s] = $temp1;
}
```

#### Output:
```
[2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97 101 103 107 109 113 127 131 137 139 149
```


Then a slightly more Raku-ish implementation based on the description in the Wikipedia article:

```perl
unit sub MAIN($limit = 150);

class Wheel {
  has $.members is rw;
  has $.length is rw;
  method extend(*@limits) {
    my @members = $.members.keys;
    for @members -> $w {
      my $n = $w + $.length;
      while $n <= @limits.all {
        $.members.set($n);
        $n += $.length;
      }
    }
    $.length = @limits.min;
  }
}

# start with W₀=({1},1)
my $wheel = Wheel.new: :members(SetHash(1)), :length(1);
my $prime = 2;
my @primes = ();

while $prime * $prime <= $limit {
  if $wheel.length < $limit {
    $wheel.extend($prime*$wheel.length, $limit);
  }
  for $wheel.members.keys.sort(-*) -> $w {
    $wheel.members.unset($prime * $w);
  }
  @primes.push: $prime;
  $prime = $prime == 2 ?? 3 !! $wheel.members.keys.grep(*>1).sort[0];
}

if $wheel.length < $limit {
  $wheel.extend($limit);
}
@primes.append:  $wheel.members.keys.grep: * != 1;
say @primes.sort;
```


The only difference in the output is that the result of \`.sort\` is a list rather than an array, so it's printed in parentheses instead of square brackets:


```
(2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97 101 103 107 109 113 127 131 137 139 149)
```
