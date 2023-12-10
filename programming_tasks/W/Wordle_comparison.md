[1]: https://rosettacode.org/wiki/Wordle_comparison

# [Wordle comparison][1]

Updated to add a proof of concept matching for similarity where commonly found on spoofing domain names.  Of course this is just the tip of the iceberg (only comparing results after decomposition) and there are way too many  [Unicode homoglyphs](https://util.unicode.org/UnicodeJsps/confusables.jsp) that can only be matched using a [lookup table/database](http://www.unicode.org/Public/security/8.0.0/confusables.txt).

```perl
# 20220216 Raku programming solution

sub wordle (\answer,\guess where [==] (answer,guess)».chars ) {

   my ($aSet, $gSet, @return) = (answer,guess)».&{ (set .comb.pairs).SetHash }

   (my \intersection = $aSet ∩ $gSet).keys».&{ @return[.key] = 'green' }
   ($aSet,$gSet)».&{ $_ ∖= intersection } # purge common subset

   for $gSet.keys.sort -> \trial { # pair 
      @return[trial.key] = 'grey';
      for $aSet.keys -> \actual { # pair
         if [eq] (trial,actual)».value {
            @return[trial.key] = 'yellow'; 
            $aSet{actual}:delete;
            last
         } 
         my @NFD = (trial,actual).map: { .value.NFD }
         if [ne] @NFD and [==] @NFD».first {
            @return[trial.key] = 'azure';
            $aSet{actual}:delete;
            last
         }
      }
   }
   @return
}

say .[0]~' vs '~.[1]~"\t"~ wordle .[0],.[1] for (
<ALLOW LOLLY>, <ROBIN ALERT>, <ROBIN SONIC>, <ROBIN ROBIN>, <BULLY LOLLY>,
<ADAPT SÅLÅD>, <Ukraine Ukraíne>, <BBAABBB BBBBBAA>, <BBAABBB AABBBAA> );
```

#### Output:
```
ALLOW vs LOLLY  yellow yellow green grey grey
ROBIN vs ALERT  grey grey grey yellow grey
ROBIN vs SONIC  grey green yellow green grey
ROBIN vs ROBIN  green green green green green
BULLY vs LOLLY  grey grey green green green
ADAPT vs SÅLÅD  grey azure grey azure yellow
Ukraine vs Ukraíne      green green green green azure green green
BBAABBB vs BBBBBAA      green green yellow yellow green yellow yellow
BBAABBB vs AABBBAA      yellow yellow yellow yellow green grey grey
```
