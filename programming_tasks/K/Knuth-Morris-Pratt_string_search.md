[1]: https://rosettacode.org/wiki/Knuth-Morris-Pratt_string_search

# [Knuth-Morris-Pratt string search][1]

Based on pseudocode found in WP ([kmp_search ](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm#Description_of_pseudocode_for_the_search_algorithm) &amp; [kmp_table](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm#Description_of_pseudocode_for_the_table-building_algorithm)). Data and output format mostly follows the Wren entry.

```perl
# 20220810 Raku programming solution

sub kmp_search (@S where *.Bool, @W where *.Bool) {

   sub kmp_table (@W where *.Bool) {
      loop (my ($pos,$cnd,@T) = 1,0,-1 ; $pos < @W.elems ; ($pos, $cnd)>>++) {
         if @W[$pos] eq @W[$cnd] {
            @T[$pos]  = @T[$cnd]
         } else {
            @T[$pos]  = $cnd;
            while $cnd ≥ 0 and @W[$pos] ne @W[$cnd] { $cnd = @T[$cnd] }
         }
      }
      @T[$pos] = $cnd;
      return @T
   }

   return gather loop (my ($j,$k,@T) = 0,0, |kmp_table @W; $j < @S.elems; ) { 
      if @W[$k] eq @S[$j] { 
         ($j, $k)>>++;   
         if $k == @W.elems { 
	    take $j - $k;
            $k = @T[$k]  
         } 
      } else {
         ($j, $k)>>++ if ($k = @T[$k]) < 0    
      }
   }
}

my @texts = [
   "GCTAGCTCTACGAGTCTA",
   "GGCTATAATGCGTA",
   "there would have been a time for such a word",
   "needle need noodle needle",
   "BharôtভাৰতBharôtভারতIndiaBhāratભારતBhāratभारतBhārataಭಾರತBhāratभारतBhāratamഭാരതംBhāratभारतBhāratभारतBharôtôଭାରତBhāratਭਾਰਤBhāratamभारतम्Bārataபாரதம்BhāratamഭാരതംBhāratadēsamభారతదేశం",
   "InhisbookseriesTheArtofComputerProgrammingpublishedbyAddisonWesleyDKnuthusesanimaginarycomputertheMIXanditsassociatedmachinecodeandassemblylanguagestoillustratetheconceptsandalgorithmsastheyarepresented",
   "Nearby farms grew a half acre of alfalfa on the dairy's behalf, with bales of all that alfalfa exchanged for milk.",
];

my @pats = ["TCTA", "TAATAAA", "word", "needle", "ഭാരതം", "put", "and", "alfalfa"];

say "text$_ = @texts[$_]" for @texts.keys ; 
say();

for @pats.keys {
   my \j = $_ < 6 ?? $_ !! $_-1 ; # for searching text5 twice
   say "Found '@pats[$_]' in 'text{j}' at indices ", kmp_search @texts[j].comb, @pats[$_].comb
}
```

#### Output:
```
text0 = GCTAGCTCTACGAGTCTA
text1 = GGCTATAATGCGTA
text2 = there would have been a time for such a word
text3 = needle need noodle needle
text4 = BharôtভাৰতBharôtভারতIndiaBhāratભારતBhāratभारतBhārataಭಾರತBhāratभारतBhāratamഭാരതംBhāratभारतBhāratभारतBharôtôଭାରତBhāratਭਾਰਤBhāratamभारतम्Bārataபாரதம்BhāratamഭാരതംBhāratadēsamభారతదేశం
text5 = InhisbookseriesTheArtofComputerProgrammingpublishedbyAddisonWesleyDKnuthusesanimaginarycomputertheMIXanditsassociatedmachinecodeandassemblylanguagestoillustratetheconceptsandalgorithmsastheyarepresented
text6 = Nearby farms grew a half acre of alfalfa on the dairy's behalf, with bales of all that alfalfa exchanged for milk.

Found 'TCTA' in 'text0' at indices (6 14)
Found 'TAATAAA' in 'text1' at indices ()
Found 'word' in 'text2' at indices (40)
Found 'needle' in 'text3' at indices (0 19)
Found 'ഭാരതം' in 'text4' at indices (68 138)
Found 'put' in 'text5' at indices (26 90)
Found 'and' in 'text5' at indices (101 128 171)
Found 'alfalfa' in 'text6' at indices (33 87)
```
