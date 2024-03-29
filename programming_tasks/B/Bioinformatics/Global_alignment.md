[1]: https://rosettacode.org/wiki/Bioinformatics/Global_alignment

# [Bioinformatics/Global alignment][1]

```perl
# 20210209 Raku programming solution

sub printCounts(\seq) {
   my $bases = seq.comb.Bag ;
   say "\nNucleotide counts for ", seq, " :";
   say $bases.kv, " and total length = ", $bases.total
}

sub stringCentipede(\s1, \s2) {
   loop ( my $offset = 0, my \S1 = $ = '' ; ; $offset++ ) {
      S1 = s1.substr: $offset ;
      with S1.index(s2.substr(0,1)) -> $p { $offset += $p } else { return False }
      return s1.chars - $offset if s2.starts-with: s1.substr: $offset
   }
}

sub deduplicate {
   my @sorted = @_.unique.sort: *.chars; # by length   
   gather while ( my $target = shift @sorted ) {
      take $target unless @sorted.grep: { .contains: $target }  
   }
}

sub shortestCommonSuperstring {
   my \ß = $ = [~] my @ss = deduplicate @_ ;           # ShortestSuper
   for @ss.permutations -> @perm {
      my \sup = $ = @perm[0];
      for @perm.rotor(2 => -1) { sup ~= @_[1].substr: stringCentipede |@_ }
      ß = sup if sup.chars < ß.chars ;
   }
   ß
}

.&shortestCommonSuperstring.&printCounts for ( 

   <TA AAG TA GAA TA>,

   <CATTAGGG ATTAG GGG TA>,

   <AAGAUGGA GGAGCGCAUC AUCGCAAUAAGGA> ,

   <ATGAAATGGATGTTCTGAGTTGGTCAGTCCCAATGTGCGGGGTTTCTTTTAGTACGTCGGGAGTGGTATTAT 
    GGTCGATTCTGAGGACAAAGGTCAAGATGGAGCGCATCGAACGCAATAAGGATCATTTGATGGGACGTTTCGTCGACAAAGT
    CTATGTTCTTATGAAATGGATGTTCTGAGTTGGTCAGTCCCAATGTGCGGGGTTTCTTTTAGTACGTCGGGAGTGGTATTATA
    TGCTTTCCAATTATGTAAGCGTTCCGAGACGGGGTGGTCGATTCTGAGGACAAAGGTCAAGATGGAGCGCATC
    AACGCAATAAGGATCATTTGATGGGACGTTTCGTCGACAAAGTCTTGTTTCGAGAGTAACGGCTACCGTCTT
    GCGCATCGAACGCAATAAGGATCATTTGATGGGACGTTTCGTCGACAAAGTCTTGTTTCGAGAGTAACGGCTACCGTC
    CGTTTCGTCGACAAAGTCTTGTTTCGAGAGTAACGGCTACCGTCTTCGATTCTGCTTATAACACTATGTTCT
    TGCTTTCCAATTATGTAAGCGTTCCGAGACGGGGTGGTCGATTCTGAGGACAAAGGTCAAGATGGAGCGCATC
    CGTAAAAAATTACAACGTCCTTTGGCTATCTCTTAAACTCCTGCTAAATGCTCGTGC
    GATGGAGCGCATCGAACGCAATAAGGATCATTTGATGGGACGTTTCGTCGACAAAGTCTTGTTTCGAGAGTAACGGCTACCGTCTTCGATT
    TTTCCAATTATGTAAGCGTTCCGAGACGGGGTGGTCGATTCTGAGGACAAAGGTCAAGATGGAGCGCATC
    CTATGTTCTTATGAAATGGATGTTCTGAGTTGGTCAGTCCCAATGTGCGGGGTTTCTTTTAGTACGTCGGGAGTGGTATTATA
    TCTCTTAAACTCCTGCTAAATGCTCGTGCTTTCCAATTATGTAAGCGTTCCGAGACGGGGTGGTCGATTCTGAGGACAAAGGTCAAGA
   >,
)
```

#### Output:
```
Nucleotide counts for TAAGAA :
(T 1 A 4 G 1) and total length = 6

Nucleotide counts for CATTAGGG :
(G 3 A 2 T 2 C 1) and total length = 8

Nucleotide counts for AAGAUGGAGCGCAUCGCAAUAAGGA :
(A 10 U 3 C 4 G 8) and total length = 25

Nucleotide counts for CGTAAAAAATTACAACGTCCTTTGGCTATCTCTTAAACTCCTGCTAAATGCTCGTGCTTTCCAATTATGTAAGCGTTCCGAGACGGGGTGGTCGATTCTGAGGACAAAGGTCAAGATGGAGCGCATCGAACGCAATAAGGATCATTTGATGGGACGTTTCGTCGACAAAGTCTTGTTTCGAGAGTAACGGCTACCGTCTTCGATTCTGCTTATAACACTATGTTCTTATGAAATGGATGTTCTGAGTTGGTCAGTCCCAATGTGCGGGGTTTCTTTTAGTACGTCGGGAGTGGTATTATA :
(C 57 G 75 A 74 T 94) and total length = 300
```
