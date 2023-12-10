[1]: https://rosettacode.org/wiki/Bioinformatics/Subsequence

# [Bioinformatics/Subsequence][1]

Chances are actually pretty small that a random 4 codon string will show up at all in a random 200 codon sequence. Bump up the sequence size to get a reasonable chance of multiple matches.

```perl
use String::Splice:ver<0.0.3+>;

my $line = 80;

my $haystack = [~] <A C G T>.roll($line * 8);

say 'Needle: ' ~ my $needle = [~] <A C G T>.roll(4);

my $these = $haystack ~~ m:g/<$needle>/;

my @match = $these.map: { .from, .pos }

printf "From: %3s to %3s\n", |$_ for @match;

my $disp = $haystack.comb.batch($line)».join.join("\n");

for @match.reverse {
    $disp.=&splice(.[1] + .[1] div $line, "\e[0m" );
    $disp.=&splice(.[0] + .[0] div $line, "\e[31m");
}

say $disp;
```


Show in custom div to better display highlighting.



Needle: TAGC

From: 159 to 163

From: 262 to 266

From: 315 to 319

From: 505 to 509

From: 632 to 636

CATATGTGACACTGACAGCTCGCGCGAAAATCCGTGTGACGGTCTGAACACTATACTATAGGCCCGGTCGGCATTTGTGG

CTCCCCAGTGGAGAGACCACTCGTCAATTGCTGACGACTTAACACAAATCGAGTCGCCCTTAGTGCCAGACGGGACTCC<span style="color: #CC0000;">T</span>

<span style="color: #CC0000;">AGC</span>AAAGGGCGGCACGTGGTGACTCCCAATATGTGAGCATGCCATCTAATTGATCTGGGGGGTTTCGCGGGAATACCTAG

GGGCGTTCTGTCCATGGATCTC<span style="color: #CC0000;">TAGC</span>CCTGCGAAGAGATACCCGCAGTGAGTTGCACGTGCAAAGAACTTGTAAC<span style="color: #CC0000;">TAGC</span>G

TATTCTGTATCCGCCGCGCGATATGCTTCTGCGGGATGTACTTCTTGTGACTAAGACTTTGTTATCCAAATTGACCAATA

TTCAACGGTCGACTCTCCGAGGCAGTATCGGTACGCCGAAAAATGGTTACTTCGGCCATACGTAACCTCTCAAGTCACGA

TTACAGCCCACGGGGGCTTACAGCA<span style="color: #CC0000;">TAGC</span>TCCAAAGACATTCCAATTGAGCTACAACGTGTTCAGTGCGGAGCAGTATCC

AGTACTCGACTGTTATGGTAAAAGGGCATCGTGATCGTTTATATTAATCATTGGGACAGGTGGTTAATGTCA<span style="color: #CC0000;">TAGC</span>TTAG
