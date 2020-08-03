[1]: https://rosettacode.org/wiki/Jaro-Winkler_Distance

# [Jaro-Winkler Distance][1]

A minor modification of the [Jaro distance](https://rosettacode.org/wiki/Jaro_distance#Raku) task entry.



using the unixdict.txt file from www.puzzlers.org

```raku
sub jaro-winkler ($s, $t) {
 
    return 0 if $s eq $t;
 
    my $s_len = + my @s = $s.comb;
    my $t_len = + my @t = $t.comb;
 
    my $match_distance = ($s_len max $t_len) div 2 - 1;
 
    my @s_matches;
    my @t_matches;
    my $matches = 0;
 
    for ^@s -> $i {
 
        my $start = 0 max $i - $match_distance;
        my $end = $i + $match_distance min ($t_len - 1);
 
        for $start .. $end -> $j {
            @t_matches[$j] and next;
            @s[$i] eq @t[$j] or next;
            @s_matches[$i] = 1;
            @t_matches[$j] = 1;
            $matches++;
            last;
        }
    }
 
    return 1 if $matches == 0;
 
    my $k              = 0;
    my $transpositions = 0;
 
    for ^@s -> $i {
        @s_matches[$i] or next;
        until @t_matches[$k] { ++$k }
        @s[$i] eq @t[$k] or ++$transpositions;
        ++$k;
    }
 
    my $prefix = 0;
 
    ++$prefix if @s[$_] eq @t[$_] for ^(min 4, $s_len, $t_len);
 
    my $jaro = ($matches / $s_len + $matches / $t_len +
        (($matches - $transpositions / 2) / $matches)) / 3;
 
    1 - ($jaro + $prefix * .1 * ( 1 - $jaro) )
}
 
 
my @words =  './unixdict.txt'.IO.slurp.words;
 
for <accomodate definately goverment occured publically recieve seperate untill wich>
   -> $word {
 
   my %result = @words.race.map: { $_ => jaro-winkler($word, $_) };
 
   say "\nClosest 5 dictionary words with a Jaro-Winkler distance < .15 from $word:";
 
   printf "%15s : %0.4f\n", .key, .value for %result.grep({ .value < .15 }).sort({+.value, ~.key}).head(5);
}
```

#### Output:
```
Closest 5 dictionary words with a Jaro-Winkler distance < .15 from accomodate:
    accommodate : 0.0182
      accordant : 0.1044
       accolade : 0.1136
      acclimate : 0.1219
    accompanist : 0.1327

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from definately:
         define : 0.0800
       definite : 0.0850
        defiant : 0.0886
     definitive : 0.1200
      designate : 0.1219

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from goverment:
         govern : 0.0667
       governor : 0.1167
      governess : 0.1175
     governance : 0.1330
       coverlet : 0.1361

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from occured:
       occurred : 0.0250
          occur : 0.0571
      occurrent : 0.0952
        occlude : 0.1056
      concurred : 0.1217

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from publically:
         public : 0.0800
    publication : 0.1327
           pull : 0.1400
       pullback : 0.1492

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from recieve:
        receive : 0.0333
        relieve : 0.0667
          reeve : 0.0762
      receptive : 0.0852
      recessive : 0.0852

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from seperate:
      desperate : 0.0708
       separate : 0.0917
      temperate : 0.1042
       selenate : 0.1167
           sept : 0.1167

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from untill:
          until : 0.0333
           till : 0.1111
     huntsville : 0.1333
        instill : 0.1357
         unital : 0.1422

Closest 5 dictionary words with a Jaro-Winkler distance < .15 from wich:
          winch : 0.0533
          witch : 0.0533
          which : 0.0600
        wichita : 0.0857
         switch : 0.1111
```
