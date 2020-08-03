[1]: https://rosettacode.org/wiki/Text_Completion

# [Text Completion][1]





### Hamming distance

```perl
sub MAIN ( Str $user_word = 'complition', Str $filename = 'words.txt' ) {
    my @s1 = $user_word.comb;
    my @listed = gather for $filename.IO.lines -> $line {
        my @s2 = $line.comb;
 
        my $correct = 100 * sum( @s1 Zeq @s2)
                          / max(+@s1,   +@s2);
 
        my $score = ( $correct >= 100            and @s1[0] eq @s2[0] ) ?? 100
                 !! ( $correct >=  80            and @s1[0] eq @s2[0] ) ?? $correct
                 !! ( $line.contains($user_word) and @s1 * 2 > @s2    ) ?? 80
                 !!                                                        0;
        take [$score, $line] if $score;
    }
 
    @listed = @listed[$_] with @listed.first: :k, { .[0] == 100 };
 
    say "{.[0].fmt('%.2f')}% {.[1]}" for @listed;
}
```

#### Output:
```
80.00% compaction
90.00% completion
81.82% completions
80.00% complexion
```


### Sorenson-Dice



Sorenson-Dice tends to return relatively low percentages even for small differences, especially for short words. We need to "lower the bar" to get any results at all. Different variations of the algorithm do or don't regularize case. This one does, though it doesn't much matter for the tested words.



Using unixdict.txt from www.puzzlers.org

```perl
sub sorenson ($phrase, %hash) {
    my $match = bigram $phrase;
    %hash.race.map: { [(2 * +($match ∩ .value) / (+$match + .value)).round(.001), .key] }
}
 
sub bigram (\these) { Bag.new( flat these.fc.words.map: { .comb.rotor(2 => -1)».join } ) }
 
# Load the dictionary
my %hash = './unixdict.txt'.IO.slurp.words.race.map: { $_ => .&bigram };
 
# Testing
for <complition inconsqual Sørenson> -> $w {
    say "\n$w:";
    .say for sorenson($w, %hash).grep(*.[0] >= .55).sort({-.[0],~.[1]}).head(10);
}
```

#### Output:
```
complition:
[0.778 completion]
[0.737 competition]
[0.737 composition]
[0.706 coalition]
[0.7 incompletion]
[0.667 complexion]
[0.667 complicity]
[0.667 decomposition]
[0.632 compilation]
[0.632 compunction]

inconsqual:
[0.609 inconsequential]
[0.588 continual]
[0.571 squall]
[0.556 conceptual]
[0.556 inconstant]

Sørenson:
[0.714 sorenson]
[0.667 benson]
[0.615 swenson]
[0.571 evensong]
[0.571 sorensen]
```
