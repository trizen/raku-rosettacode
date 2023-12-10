[1]: https://rosettacode.org/wiki/Count_how_many_vowels_and_consonants_occur_in_a_string

# [Count how many vowels and consonants occur in a string][1]

Note that the task **does not** ask for the **total count** of vowels and consonants, but for **how many** occur.

```perl
my @vowels     = <a e i o u>;
my @consonants = <b c d f g h j k l m n p q r s t v w x y z>;

sub letter-check ($string) {
    my $letters = $string.lc.comb.Set;
    "{sum $letters{@vowels}} vowels and {sum $letters{@consonants}} consonants occur in the string \"$string\"";
}

say letter-check "Forever Ring Programming Language";
```

#### Output:
```
5 vowels and 8 consonants occur in the string "Forever Ring Programming Language"
```
