[1]: https://rosettacode.org/wiki/String_matching

# [String matching][1]

Using string methods:

```perl
$haystack.starts-with($needle)  # True if $haystack starts with $needle
$haystack.contains($needle)     # True if $haystack contains $needle
$haystack.ends-with($needle)    # True if $haystack ends with $needle
```


Using regexes:

```perl
so $haystack ~~ /^ $needle  /  # True if $haystack starts with $needle
so $haystack ~~ /  $needle  /  # True if $haystack contains $needle
so $haystack ~~ /  $needle $/  # True if $haystack ends with $needle
```


Using `substr`:

```perl
substr($haystack, 0, $needle.chars) eq $needle  # True if $haystack starts with $needle
substr($haystack, *-$needle.chars) eq $needle   # True if $haystack ends with $needle
```


Bonus task:

```perl
$haystack.match($needle, :g)».from;  # List of all positions where $needle appears in $haystack
```