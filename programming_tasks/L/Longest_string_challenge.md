[1]: https://rosettacode.org/wiki/Longest_string_challenge

# [Longest string challenge][1]



```perl
my $l = '';  # Sample longest string seen.
my $a = '';  # Accumulator to save longest strings.

while get() -> $s {
   my $n = "$s\n";
   if $n.substr($l.chars) {     # Is new string longer?
       $a = $l = $n;            # Reset accumulator.
   }
   elsif !$l.substr($n.chars) { # Same length?
      $a ~= $n;                 # Accumulate it.
   }
}
print $a;
```


Given the example input, returns:


```
ccc
ddd
ggg
```
