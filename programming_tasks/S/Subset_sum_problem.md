[1]: http://rosettacode.org/wiki/Subset_sum_problem

# [Subset sum problem][1]

```perl
my @pairs =
    alliance => -624, archbishop => -915, balm => 397, bonnet => 452,
    brute => 870, centipede => -658, cobol => 362, covariate => 590,
    departure => 952, deploy => 44, diophantine => 645, efferent => 54,
    elysee => -326, eradicate => 376, escritoire => 856, exorcism => -983,
    fiat => 170, filmy => -874, flatworm => 503, gestapo => 915,
    infra => -847, isis => -982, lindholm => 999, markham => 475,
    mincemeat => -880, moresby => 756, mycenae => 183, plugging => -266,
    smokescreen => 423, speakeasy => -745, vein => 813;
my @weights = @pairs».value;
my %name = @pairs.hash.invert;
 
for 1..^@weights -> $n {
    given @weights.combinations($n).first({ 0 == [+] @^comb }) {
	when .so { say "Length $n: ", .map: {%name{$_}} }
	default  { say "Length $n: (none)" }
    }
}
```

#### Output:
```
Length 1: (none)
Length 2: archbishop gestapo
Length 3: centipede markham mycenae
Length 4: alliance balm deploy mycenae
Length 5: alliance brute covariate deploy mincemeat
Length 6: alliance archbishop balm deploy gestapo mycenae
Length 7: alliance archbishop bonnet cobol departure exorcism moresby
Length 8: alliance archbishop balm bonnet fiat flatworm isis lindholm
Length 9: alliance archbishop balm bonnet brute covariate eradicate mincemeat plugging
Length 10: alliance archbishop balm bonnet brute centipede cobol departure deploy mincemeat
Length 11: alliance archbishop balm bonnet brute centipede cobol departure infra moresby speakeasy
Length 12: alliance archbishop balm bonnet brute centipede cobol covariate diophantine efferent elysee infra
Length 13: alliance archbishop balm bonnet brute centipede cobol covariate departure efferent eradicate filmy isis
Length 14: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy elysee filmy markham speakeasy
Length 15: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy elysee exorcism flatworm infra mycenae
Length 16: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine elysee exorcism filmy gestapo infra
Length 17: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine exorcism isis mincemeat mycenae plugging vein
Length 18: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee exorcism filmy isis mycenae vein
Length 19: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee eradicate exorcism fiat infra isis smokescreen
Length 20: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee eradicate exorcism gestapo infra isis smokescreen speakeasy
Length 21: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee eradicate exorcism flatworm infra lindholm mincemeat plugging speakeasy
Length 22: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee eradicate escritoire exorcism fiat filmy flatworm mincemeat plugging speakeasy
Length 23: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee eradicate escritoire exorcism infra isis mincemeat moresby mycenae smokescreen speakeasy
Length 24: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine efferent elysee exorcism filmy gestapo infra markham mincemeat moresby mycenae plugging smokescreen speakeasy
Length 25: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine eradicate exorcism fiat filmy flatworm infra isis lindholm markham mincemeat moresby mycenae plugging speakeasy
Length 26: alliance archbishop balm bonnet brute centipede cobol covariate departure deploy diophantine elysee eradicate escritoire exorcism fiat filmy gestapo infra isis markham mincemeat mycenae plugging speakeasy vein
Length 27: alliance archbishop balm bonnet brute centipede covariate departure deploy efferent elysee eradicate escritoire exorcism fiat filmy flatworm infra isis lindholm markham mincemeat moresby mycenae plugging smokescreen speakeasy
```