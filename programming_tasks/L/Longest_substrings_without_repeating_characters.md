[1]: https://rosettacode.org/wiki/Longest_substrings_without_repeating_characters

# [Longest substrings without repeating characters][1]

Detects **any** character when checking for repeated characters, even multi-byte composite characters, control sequences and whitespace.



Note that there is no requirement that the substrings be unique, only that each has no repeated characters internally.



Not going to bother handling arrays since an array is not a string, and the task description **specifically** says 'Given a string'.

```perl
sub abbreviate ($_) { .chars > 80 ?? "(abbreviated)\n" ~ .substr(0,35) ~ ' ... ' ~ .substr(*-35) !! $_ }
 
sub longest ($string) {
   return 0 => [] unless $string.chars;
   my ($start, $end, @substr) = 0, 0;
   while ++$end < $string.chars {
       my $sub = $string.substr($start, $end - $start);
       if $sub.contains: my $next = $string.substr: $end, 1 {
           @substr[$end - $start].push($sub) if $end - $start >= @substr.end;
           $start += 1 + $sub.index($next);
       }
   }
   @substr[$end - $start].push: $string.substr($start);
   @substr.pairs.tail
}
 
# Testing
say "\nOriginal string: {abbreviate $_}\nLongest substring(s) chars: ", .&longest
 
# Standard tests
for flat qww< xyzyabcybdfd xyzyab zzzzz a '' >,
 
# multibyte Unicode
< 👒🎩🎓👩‍👩‍👦‍👦🧢🎓👨‍👧‍👧👒👩‍👩‍👦‍👦🎩🎓👒🧢 α⊆϶α϶ >,
 
# check a file
slurp 'unixdict.txt';
```

#### Output:
```
Original string: xyzyabcybdfd
Longest substring(s) chars: 5 => [zyabc cybdf]

Original string: xyzyab
Longest substring(s) chars: 4 => [zyab]

Original string: zzzzz
Longest substring(s) chars: 1 => [z z z z z]

Original string: a
Longest substring(s) chars: 1 => [a]

Original string: 
Longest substring(s) chars: 0 => []

Original string: 👒🎩🎓👩‍👩‍👦‍👦🧢🎓👨‍👧‍👧👒👩‍👩‍👦‍👦🎩🎓👒🧢
Longest substring(s) chars: 6 => [🧢🎓👨‍👧‍👧👒👩‍👩‍👦‍👦🎩]

Original string: α⊆϶α϶
Longest substring(s) chars: 3 => [α⊆϶ ⊆϶α]

Original string: (abbreviated)
10th
1st
2nd
3rd
4th
5th
6th
7th
8t ... rian
zounds
zucchini
zurich
zygote

Longest substring(s) chars: 15 => [mbowel
disgrunt]
```
