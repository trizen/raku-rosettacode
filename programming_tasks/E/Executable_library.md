[1]: http://rosettacode.org/wiki/Executable_library

# [Executable library][1]

The library can be written as a module:

```perl6
module Hailstone {
    our sub hailstone($n) is export {
	$n, { $_ %% 2 ?? $_ div 2 !! $_ * 3 + 1 } ... 1
    }
}
 
sub MAIN {
    say "hailstone(27) = {.[^4]} [...] {.[*-4 .. *-1]}" given Hailstone::hailstone 27;
}
```


It can be run with:

```text
$ perl6 Hailstone.pm
```

#### Output:
```
hailstone(27) = 27 82 41 124 [...] 8 4 2 1
```


It can then be used with a program such as:

```perl6
use Hailstone;
my %score; %score{hailstone($_).elems}++ for 1 .. 100_000;
say "Most common length is {.key}, occurring {.value} times." given max :by(*.value), %score;
```


Called with a command line as:


#### Output:
```
$ PERL6LIB=. perl6 test-hailstone.p6
```


The environment variable PERL6LIB might be necessary if the file Hailstone.pm is not in the standard library path for Perl 6.