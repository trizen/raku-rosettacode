[1]: https://rosettacode.org/wiki/Four_is_the_number_of_letters_in_the_...

# [Four is the number of letters in the ...][1]

Uses the Lingua::EN::Numbers::Cardinal module to generate both cardinal and ordinal numbers. This module places commas in number words between 3-orders-of-magnitude clusters. E.G. `12345678.&ordinal` becomes: twelve million, three hundred forty-five thousand, six hundred seventy-eighth. Uses a custom 'no-commas' routine to filter them out for accurate character counts. Generates the 'sentence' lazily so only the words needed are ever calculated and reified.

```perl
use Lingua::EN::Numbers::Cardinal;
 
my $index = 1;
my @sentence = flat 'Four is the number of letters in the first word of this sentence, '.words,
  { @sentence[$index++].&alpha.&cardinal, 'in', 'the', |($index.&ordinal.&no-commas~',').words } ... * ;
 
sub alpha ( $str ) { $str.subst(/\W/, '', :g).chars }
sub no-commas ( $str ) { $str.subst(',', '', :g) }
sub count ( $index ) { @sentence[^$index].join(' ').chars ~ " characters in the sentence, up to and including this word.\n" }
 
say 'First 201 word lengths in the sequence:';
put ' ', map { @sentence[$_].&alpha.fmt("%2d") ~ (((1+$_) %% 25) ?? "\n" !! '') }, ^201;
say 201.&count;
 
for 1e3, 1e4, 1e5, 1e6, 1e7 {
    say "{.&ordinal.tc} word, '{@sentence[$_ - 1]}', has {@sentence[$_ - 1].&alpha} characters. ", .&count
}
```

#### Output:
```
First 201 word lengths in the sequence:
  4  2  3  6  2  7  2  3  5  4  2  4  8  3  2  3  6  5  2  3  5  3  2  3  6
  3  2  3  5  5  2  3  5  3  2  3  7  5  2  3  6  4  2  3  5  4  2  3  5  3
  2  3  8  4  2  3  7  5  2  3 10  5  2  3 10  3  2  3  9  5  2  3  9  3  2
  3 11  4  2  3 10  3  2  3 10  5  2  3  9  4  2  3 11  5  2  3 12  3  2  3
 11  5  2  3 12  3  2  3 11  5  2  3 11  3  2  3 13  5  2  3 12  4  2  3 11
  4  2  3  9  3  2  3 11  5  2  3 12  4  2  3 11  5  2  3 12  3  2  3 11  5
  2  3 11  5  2  3 13  4  2  3 12  3  2  3 11  5  2  3  8  3  2  3 10  4  2
  3 11  3  2  3 10  5  2  3 11  4  2  3 10  4  2  3 10  3  2  3 12  5  2  3
 11
1203 characters in the sentence, up to and including this word.

One thousandth word, 'in', has 2 characters. 6249 characters in the sentence, up to and including this word.

Ten thousandth word, 'in', has 2 characters. 64097 characters in the sentence, up to and including this word.

One hundred thousandth word, 'one', has 3 characters. 659455 characters in the sentence, up to and including this word.

One millionth word, 'the', has 3 characters. 7113560 characters in the sentence, up to and including this word.

Ten millionth word, 'thousand', has 8 characters. 70995729 characters in the sentence, up to and including this word.
```