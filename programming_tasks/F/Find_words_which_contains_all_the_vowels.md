[1]: https://rosettacode.org/wiki/Find_words_which_contains_all_the_vowels

# [Find words which contains all the vowels][1]

Yet another "Filter a word list" task.

```perl
put +.words, " words found:\n", $_ with 'unixdict.txt'.IO.words\
  .grep({ .chars > 10 and all(.comb.Bag<a e i o u>) == 1 })\
  .batch(5)».fmt('%-13s').join: "\n";
```

#### Output:
```
25 words found:
ambidextrous  bimolecular   cauliflower   communicable  communicate  
consanguine   consultative  countervail   exclusionary  grandiloquent
importunate   incommutable  incomputable  insupportable loudspeaking 
malnourished  mensuration   oneupmanship  pandemonium   permutation  
perturbation  portraiture   praseodymium  stupefaction  sulfonamide 
```
