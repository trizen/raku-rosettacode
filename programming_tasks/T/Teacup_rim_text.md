[1]: https://rosettacode.org/wiki/Teacup_rim_text

# [Teacup rim text][1]

There doesn't seem to be any restriction that the word needs to consist only of lowercase letters, so words of any case are included. Since the example code specifically shows the example words (TEA, EAT, ATE) in uppercase, I elected to uppercase the found words.



As the specs keep changing, this version will accept ANY text file as its dictionary and accepts parameters to configure the minimum number of characters in a word to consider and whether to allow mono-character words.



Defaults to unixdict.txt, minimum 3 characters and mono-character 'words' disallowed. Feed a file name to use a different word list, an integer to --min-chars and/or a truthy value to --mono to allow mono-chars.

```perl
my %*SUB-MAIN-OPTS = :named-anywhere;
 
unit sub MAIN ( $dict = 'unixdict.txt', :$min-chars = 3, :$mono = False );
 
my %words;
$dict.IO.slurp.words.map: { .chars < $min-chars ?? (next) !! %words{.uc.comb.sort.join}.push: .uc };
 
my @teacups;
my %seen;
 
for %words.values -> @these {
    next if !$mono && @these < 2;
    MAYBE: for @these {
        my $maybe = $_;
        next if %seen{$_};
        my @print;
        for ^$maybe.chars {
            if $maybe ∈ @these {
                @print.push: $maybe;
                $maybe = $maybe.comb.list.rotate.join;
            } else {
                @print = ();
                next MAYBE
            }
        }
        if @print.elems {
            @teacups.push: @print;
            %seen{$_}++ for @print;
        }
    }
}
 
say .unique.join(", ") for sort @teacups;
```


Command line: `perl6 teacup.p6`


#### Output:
```
APT, PTA, TAP
ARC, RCA, CAR
ATE, TEA, EAT
```


Command line: `perl6 teacup.p6 --mono=1`


#### Output:
```
AAA
APT, PTA, TAP
ARC, RCA, CAR
ATE, TEA, EAT
III
```


words.txt file from [https://github.com/dwyl/english-words](https://github.com/dwyl/english-words)



Command line: `perl6 teacup.p6 words.txt --min-chars=4 --mono=Allow`


#### Output:
```
AAAA
AAAAAA
ADAD, DADA
ADAR, DARA, ARAD, RADA
AGAG, GAGA
ALIT, LITA, ITAL, TALI
AMAN, MANA, ANAM, NAMA
AMAR, MARA, ARAM, RAMA
AMEL, MELA, ELAM, LAME
AMEN, MENA, ENAM, NAME
AMOR, MORA, ORAM, RAMO
ANAN, NANA
ANIL, NILA, ILAN, LANI
ARAR, RARA
ARAS, RASA, ASAR, SARA
ARIS, RISA, ISAR, SARI
ASEL, SELA, ELAS, LASE
ASER, SERA, ERAS, RASE
DENI, ENID, NIDE, IDEN
DOLI, OLID, LIDO, IDOL
EGOR, GORE, OREG, REGO
ENOL, NOLE, OLEN, LENO
ESOP, SOPE, OPES, PESO
ISIS, SISI
MMMM
MORO, OROM, ROMO, OMOR
OOOO
```
