[1]: https://rosettacode.org/wiki/Find_words_with_alternating_vowels_and_consonants

# [Find words with alternating vowels and consonants][1]

Sigh. Yet another "Filter a word list" task. In a effort to make it a *little* more interesting, rather than just doing a one-liner, build a grammar and use that to filter.

```perl
grammar VOWCON {
    token       TOP { <|w> <vowel>? ( <consonant> <vowel> )* <consonant>? <|w> }
    token     vowel { <[aeiou]> }
    token consonant { <[a..z] - [aeiou]> }
}
 
say ( grep { VOWCON.parse: .lc }, grep { .chars > 9 }, 'unixdict.txt'.IO.words ).batch(6)».fmt('%-15s').join: "\n";
```

#### Output:
```
aboriginal      apologetic      bimolecular     borosilicate    calorimeter     capacitate     
capacitive      capitoline      capitulate      caricature      colatitude      coloratura     
colorimeter     debilitate      decelerate      decolonize      definitive      degenerate     
deliberate      demodulate      denominate      denotative      deregulate      desiderata     
desideratum     dilapidate      diminutive      epigenetic      facilitate      hemosiderin    
heretofore      hexadecimal     homogenate      inoperative     judicature      latitudinal    
legitimate      lepidolite      literature      locomotive      manipulate      metabolite     
nicotinamide    oratorical      paragonite      pejorative      peridotite      peripatetic    
polarimeter     recitative      recuperate      rehabilitate    rejuvenate      remunerate     
repetitive      reticulate      savonarola      similitude      solicitude      tananarive     
telekinesis     teratogenic     topologize      unilateral      unimodular      uninominal     
verisimilitude
```
