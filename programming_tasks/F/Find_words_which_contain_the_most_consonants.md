[1]: https://rosettacode.org/wiki/Find_words_which_contain_the_most_consonants

# [Find words which contain the most consonants][1]

Hmm. Depends on how you define "most".

```perl
unit sub MAIN ($min = 11);
my @vowel = <a e i o u>;
my $vowel = rx/ @vowel /;
my @consonant = ('a'..'z').grep: { $_ ∉ @vowel };
my $consonant = rx/ @consonant /;

say "Minimum characters in a word: $min";

say "Number found, consonant count and list of words with the largest absolute number of unique, unrepeated consonants:";
say +.value, ' @ ', $_ for 'unixdict.txt'.IO.words.grep( {.chars >= $min and so all(.comb.Bag{@consonant}) <= 1} )
    .classify( { +.comb(/$consonant/) } ).sort(-*.key).head(2);

say "\nNumber found, ratio and list of words with the highest ratio of unique, unrepeated consonants to vowels:";
say +.value, ' @ ', $_  for 'unixdict.txt'.IO.slurp.words.grep( {.chars >= $min and so all(.comb.Bag{@consonant}) <= 1} )
    .classify( { +.comb(/$vowel/) ?? (+.comb(/$consonant/) / +.comb(/$vowel/) ) !! Inf } ).sort(-*.key).head(3);
```

#### Output:
```
Minimum characters in a word: 11
Number found, consonant count and list of words with the largest absolute number of unique, unrepeated consonants:
1 @ 9 => [comprehensible]
39 @ 8 => [administrable anthropology blameworthy bluestocking boustrophedon bricklaying chemisorption christendom claustrophobia compensatory comprehensive counterexample demonstrable disciplinary discriminable geochemistry hypertensive indecipherable indecomposable indiscoverable lexicography manslaughter misanthropic mockingbird monkeyflower neuropathology paralinguistic pharmacology pitchblende playwriting shipbuilding shortcoming springfield stenography stockholder switchblade switchboard switzerland thunderclap]

Number found, ratio and list of words with the highest ratio of unique, unrepeated consonants to vowels:
14 @ 2.666667 => [blameworthy bricklaying christendom mockingbird pitchblende playwriting shortcoming springfield stenography stockholder switchblade switchboard switzerland thunderclap]
13 @ 2 => [anthropology bluestocking compensatory demonstrable disciplinary geochemistry hypertensive lexicography manslaughter misanthropic monkeyflower pharmacology shipbuilding]
1 @ 1.8 => [comprehensible]
```


Character count of 11 minimum is rather arbitrary. If we feed it a character count minimum of 5, the first one returns a pretty similar answer, 8 additional words at 8 consonants (all with 10 characters.)



The second would actually come up with some infinite ratios (since we somewhat bogusly don't count 'y' as a vowel.):


```
Minimum characters in a word: 5
Number found, consonant count and list of words with the largest absolute number of unique, unrepeated consonants:
1 @ 9 => [comprehensible]
47 @ 8 => [administrable anthropology bankruptcy blacksmith blameworthy bluestocking boustrophedon bricklaying chemisorption christendom claustrophobia compensatory comprehensive counterexample crankshaft demonstrable disciplinary discriminable geochemistry hypertensive indecipherable indecomposable indiscoverable lexicography lynchburg manslaughter misanthropic mockingbird monkeyflower neuropathology paralinguistic pharmacology pitchblende playwright playwriting shipbuilding shortcoming sprightly springfield stenography stockholder strickland stronghold switchblade switchboard switzerland thunderclap]

Number found, ratio and list of words with the highest ratio of unique, unrepeated consonants to vowels:
6 @ Inf => [crypt glyph lymph lynch nymph psych]
2 @ 8 => [lynchburg sprightly]
5 @ 7 => [klystron mcknight schwartz skylight splotchy]
```
