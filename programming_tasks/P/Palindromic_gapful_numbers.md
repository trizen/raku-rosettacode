[1]: https://rosettacode.org/wiki/Palindromic_gapful_numbers

# [Palindromic gapful numbers][1]

```raku
# Infinite lazy iterator to generate palindromic "gap" numbers
my @npal = flat [ ^10 ], [ ^10 .map: * x 2 ],
  {
    sink @^previous;
    my \count = +@^penultimate;
    [ flat ^10 .map: -> \digit { digit xx count Z~ @penultimate Z~ digit xx count } ]
  } … *;
 
# Individual lazy palindromic gapful number iterators for each start/end digit
my @gappal = (1..9).map: -> \digit {
    my \divisor = digit + 10 * digit;
    @npal.map: -> \this { next unless (my \test = digit ~ this ~ digit) %% divisor; test }
}
 
# Display
for "(Required) First 20 gapful palindromes:",              ^20, 7
    ,"\n(Required) 86th through 100th:",                 85..99, 8
    ,"\n(Optional) 991st through 1,000th:",            990..999, 10
    ,"\n(Extra stretchy) 9,995th through 10,000th:", 9994..9999, 12
    ,"\n(Meh) 100,000th:",                                99999, 14
  -> $caption, $range, $fmt {
    my $now = now;
    say $caption;
    put "$_: " ~ @gappal[$_-1][|$range].fmt("%{$fmt}s") for 1..9;
    say round( now - $now, .001 ), " seconds";
}
```
