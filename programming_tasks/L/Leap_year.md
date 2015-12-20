[1]: http://rosettacode.org/wiki/Leap_year

# [Leap year][1]

```perl
say "$year is a {Date.is-leap-year($year) ?? 'leap' !! 'common'} year."
```


In Rakudo 2010.07, `Date.is-leap-year` is implemented as

```perl
multi method is-leap-year($y = $!year) {
    $y %% 4 and not $y %% 100 or $y %% 400
}
```