[1]: https://rosettacode.org/wiki/AKS_test_for_primes

# [AKS test for primes][1]

The expansions are generated similarly to how most FP languages generate sequences that resemble Pascal's triangle, using a zipwith meta-operator (Z) with subtraction, applied between two lists that add a 0 on either end to the prior list. Here we define a constant infinite sequence using the `...` sequence operator with a "whatever" endpoint. In fact, the second term `[1,-1]` could have been generated from the first term, but we put it in there for documentation so the reader can see what direction things are going.



The `polyprime` function pretty much reads like the original description. Is it "so" that the p'th expansion's coefficients are all divisible by p? The `.[1 ..^ */2]` slice is done simply to weed out divisions by 1 or by factors we've already tested (since the coefficients are symmetrical in terms of divisibility). If we wanted to write `polyprime` even more idiomatically, we could have made it another infinite constant list that is just a mapping of the first list, but we decided that would just be showing off. `:-)`

```raku
constant expansions = [1], [1,-1], -> @prior { [|@prior,0 Z- 0,|@prior] } ... *;
 
sub polyprime($p where 2..*) { so expansions[$p].[1 ..^ */2].all %% $p }
 
# Showing the expansions:
 
say ' p: (x-1)ᵖ';
say '-----------';
 
sub super ($n) {
    $n.trans: '0123456789'
           => '⁰¹²³⁴⁵⁶⁷⁸⁹';
}
 
for ^13 -> $d {
    say $d.fmt('%2i: '), (
        expansions[$d].kv.map: -> $i, $n {
            my $p = $d - $i;
            [~] gather {
                take < + - >[$n < 0] ~ ' ' unless $p == $d;
                take $n.abs                unless $p == $d > 0;
                take 'x'                   if $p > 0;
                take super $p - $i         if $p > 1;
            }
        }
    )
}
 
#  And testing the function:
 
print "\nPrimes up to 100:\n  { grep &polyprime, 2..100 }\n";
```

#### Output:
```
 p: (x-1)ᵖ
-----------
 0: 1
 1: x - 1
 2: x² - 2x + 1
 3: x³ - 3x² + 3x - 1
 4: x⁴ - 4x³ + 6x² - 4x + 1
 5: x⁵ - 5x⁴ + 10x³ - 10x² + 5x - 1
 6: x⁶ - 6x⁵ + 15x⁴ - 20x³ + 15x² - 6x + 1
 7: x⁷ - 7x⁶ + 21x⁵ - 35x⁴ + 35x³ - 21x² + 7x - 1
 8: x⁸ - 8x⁷ + 28x⁶ - 56x⁵ + 70x⁴ - 56x³ + 28x² - 8x + 1
 9: x⁹ - 9x⁸ + 36x⁷ - 84x⁶ + 126x⁵ - 126x⁴ + 84x³ - 36x² + 9x - 1
10: x¹⁰ - 10x⁹ + 45x⁸ - 120x⁷ + 210x⁶ - 252x⁵ + 210x⁴ - 120x³ + 45x² - 10x + 1
11: x¹¹ - 11x¹⁰ + 55x⁹ - 165x⁸ + 330x⁷ - 462x⁶ + 462x⁵ - 330x⁴ + 165x³ - 55x² + 11x - 1
12: x¹² - 12x¹¹ + 66x¹⁰ - 220x⁹ + 495x⁸ - 792x⁷ + 924x⁶ - 792x⁵ + 495x⁴ - 220x³ + 66x² - 12x + 1

Primes up to 100:
  2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97
```