[1]: https://rosettacode.org/wiki/Multisplit

# [Multisplit][1]



```perl
sub multisplit($str, @seps) { $str.split: / ||@seps /, :v }

my @chunks = multisplit 'a!===b=!=c==d', < == != = >;

# Print the strings.
say @chunks».Str.raku;

# Print the positions of the separators.
for grep Match, @chunks -> $s {
    say "{$s.fmt: '%2s'} from {$s.from.fmt: '%2d'} to {$s.to.fmt: '%2d'}";
}
```

#### Output:
```
("a", "!=", "", "==", "b", "=", "", "!=", "c", "==", "d")
!= from  1 to  3
== from  3 to  5
 = from  6 to  7
!= from  7 to  9
== from 10 to 12
```


Using the array `@seps` in a pattern automatically does alternation.
By default this would do longest-term matching (that is, `|` semantics), but we can force it to do left-to-right matching by embedding the array in a short-circuit alternation (that is, `||` semantics).
As it happens, with the task's specified list of separators, it doesn't make any difference.



Raku automatically returns Match objects that will stringify to the matched pattern, but can also be interrogated for their match positions, as illustrated above by post-processing the results two different ways.
