[1]: https://rosettacode.org/wiki/Words_containing_"the"_substring

# [Words containing &quot;the&quot; substring][1]

A trivial modification of the [ABC words](https://rosettacode.org/wiki/ABC_words#Raku) task.

```perl
put 'unixdict.txt'.IO.words».fc.grep({ (.chars > 11) && (.contains: 'the') })\
    .&{"{+$_} words:\n  " ~ .batch(8)».fmt('%-17s').join: "\n  "};
```

#### Output:
```
32 words:
  authenticate      chemotherapy      chrysanthemum     clothesbrush      clotheshorse      eratosthenes      featherbedding    featherbrain     
  featherweight     gaithersburg      hydrothermal      lighthearted      mathematician     neurasthenic      nevertheless      northeastern     
  northernmost      otherworldly      parasympathetic   physiotherapist   physiotherapy     psychotherapeutic psychotherapist   psychotherapy    
  radiotherapy      southeastern      southernmost      theoretician      weatherbeaten     weatherproof      weatherstrip      weatherstripping 
```
