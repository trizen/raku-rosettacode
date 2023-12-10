[1]: https://rosettacode.org/wiki/Append_numbers_at_same_position_in_strings

# [Append numbers at same position in strings][1]

Various manipulations.

```perl
my @lists = 1..9, 10..18, 19..27;
put [Z~] @lists;         # the task
put [Z~] @lists».flip;   # each component reversed
put [RZ~] @lists;        # in reversed order
put ([Z~] @lists)».flip; # reversed components in reverse order
```

#### Output:
```
11019 21120 31221 41322 51423 61524 71625 81726 91827
10191 21102 32112 43122 54132 65142 76152 87162 98172
19101 20112 21123 22134 23145 24156 25167 26178 27189
91011 02112 12213 22314 32415 42516 52617 62718 72819
```
