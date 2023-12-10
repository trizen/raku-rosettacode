[1]: https://rosettacode.org/wiki/Executable_library

# [Executable library][1]


The library can be written as a module:

```perl
module Hailstone {
    our sub hailstone($n) is export {
	$n, { $_ %% 2 ?? $_ div 2 !! $_ * 3 + 1 } ... 1
    }
}

sub MAIN {
    say "hailstone(27) = {.[^4]} [...] {.[*-4 .. *-1]}" given Hailstone::hailstone 27;
}
```


It can be run with:

```shell
$ raku Hailstone.rakumod
```

#### Output:
```
hailstone(27) = 27 82 41 124 [...] 8 4 2 1
```


It can then be used with a program such as:

```perl
use Hailstone;
my %score;
(1 .. 100_000).race.map: { %score{hailstone($_).elems}++ };
say "Most common length is {.key}, occurring {.value} times." given max :by(*.value), %score;
```


Called with a command line as:


```
$ RAKULIB=. raku test-hailstone.raku
```


The environment variable RAKULIB might be necessary if the file Hailstone.rakumod is not in the standard library path for Raku.
