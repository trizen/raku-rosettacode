[1]: https://rosettacode.org/wiki/Markov_chain_text_generator

# [Markov chain text generator][1]

```perl
unit sub MAIN ( :$text=$*IN, :$n=2, :$words=100, );
 
sub add-to-dict ( $text, :$n=2, ) {
    my @words = $text.words;
    my @prefix = @words.rotor: $n => -$n+1;
 
    (%).push: @prefix Z=> @words[$n .. *]
}
 
my %dict = add-to-dict $text, :$n;
my @start-words = %dict.keys.pick.words;
my @generated-text = lazy |@start-words, { %dict{ "@_[ *-$n .. * ]" }.pick } ...^ !*.defined;
 
put @generated-text.head: $words;
 
```

#### Output:
```
>perl6 markov.p6 <alice_oz.txt --n=3 --words=200
Scarecrow. He can't hurt the straw. Do let me carry that basket for you. I shall not mind it, for I can't get tired. I'll tell you what I think, said the little man. Give me two or three pairs of tiny white kid gloves: she took up the fan and gloves, and, as the Lory positively refused to tell its age, there was no use in saying anything more till the Pigeon had finished. 'As if it wasn't trouble enough hatching the eggs,' said the Pigeon; 'but I must be very careful. When Oz gives me a heart of course I needn't mind so much. They were obliged to camp out that night under a large tree in the wood,' continued the Pigeon, raising its voice to a whisper. He is more powerful than they themselves, they would surely have destroyed me. As it was, I lived in deadly fear of them for many years; so you can see for yourself. Indeed, a jolly little clown came walking toward them, and Dorothy could see that in spite of all her coaxing. Hardly knowing what she did, she picked up a little bit of stick, and held it out to
```