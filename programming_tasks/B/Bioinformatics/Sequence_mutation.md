[1]: https://rosettacode.org/wiki/Bioinformatics/Sequence_mutation

# [Bioinformatics/Sequence mutation][1]

Unweighted mutations at this point. The mutated DNA strand has a "diff" operation performed on it which (in this specific case) renders the mutated base in lower case so it may be picked out more easily.






```perl
my @bases = <A C G T>;
 
# The DNA strand
my $dna = @bases.roll(200).join;
 
 
# The Task
put "ORIGINAL DNA STRAND:";
put pretty $dna;
put "\nTotal bases: ", +my $bases = $dna.comb.Bag;
put $bases.sort( ~*.key ).join: "\n";
 
put "\nMUTATED DNA STRAND:";
my $mutate = $dna.&mutate(10);
put pretty diff $dna, $mutate;
put "\nTotal bases: ", +my $mutated = $mutate.comb.Bag;
put $mutated.sort( ~*.key ).join: "\n";
 
 
# Helper subs
sub pretty ($string, $wrap = 50) {
    $string.comb($wrap).map( { sprintf "%8d: %s", $++ * $wrap, $_ } ).join: "\n"
}
 
sub mutate ($dna is copy, $count = 1) {
    $dna.substr-rw((^$dna.chars).roll, 1) = @bases.roll for ^$count;
    $dna
}
 
sub diff ($orig, $repl) {
    ($orig.comb Z $repl.comb).map( -> ($o, $r) { $o eq $r ?? $o !! $r.lc }).join
}
```

#### Output:
```
ORIGINAL DNA STRAND:
       0: ACGGATAGACCGTTCCTGCAAGCTGGTACGGTTCGAATGTTGACCTTATT
      50: CTCCGCAGCGCACTACCCGATCGGGTAACGTACTCTATATGATGCCTATT
     100: TTCCCCGCCTTACATCGGCGATCAATGTTCTTTTACGCTAACTAGGCGCA
     150: CGTCGTGCCTTACCGAGAGCCAGTTCGAAATCGTGCTGAAAATATCTGGA

Total bases: 200
A       45
C       55
G       45
T       55

MUTATED DNA STRAND:
       0: ACGGATAGcCCGTTCCTGCAAGCTGGTACGGTTCGAATGTTGACCTTATT
      50: CTCCGCAGCGCACTACCCGATCGGGTcACtcACTCTATATGAcGCCTAaT
     100: TTCCCCGCCTTACATCGGCGATCAATGTTCTTTTACGCTAACTAGGCGCA
     150: CGTCGTGCCTTACCcAGAGCCAGTTCGAAATCGTGCTGAAAATATCTGGA

Total bases: 200
A       44
C       60
G       43
T       53
```
