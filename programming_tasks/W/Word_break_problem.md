[1]: https://rosettacode.org/wiki/Word_break_problem

# [Word break problem][1]





This implementation does not necessarily find *every* combination, it returns the one with the longest matching tokens.

```perl
my @words = <a bc abc cd b>;
my $regex = @words.join('|');

put "$_: ", word-break($_) for <abcd abbc abcbcd acdbc abcdd>;

sub word-break (Str $word) { ($word ~~ / ^ (<$regex>)+ $ /)[0] // "Not possible" }
```

#### Output:
```
abcd: a b cd
abbc: a b bc
abcbcd: abc b cd
acdbc: a cd bc
abcdd: Not possible
```
