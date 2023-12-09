[1]: https://rosettacode.org/wiki/Selection_bias_in_clinical_sciences

# [Selection bias in clinical sciences][1]

```perl
# 20221025 Raku programming solution

enum <UNTREATED REGULAR IRREGULAR>;
my \DOSE_FOR_REGULAR = 100;
my ($nSubjects,$duration,$interval) = 10000, 180, 30;
my (@dosage,@category,@hadcovid) := (0,UNTREATED,False)>>.&{($_ xx $nSubjects).Array};

sub update($pCovid=1e-3, $pStartTreatment=5e-3, $pRedose=¼, @dRange=<3 6 9>) {
   for 0 ..^ @dosage.elems -> \i { 
      unless @hadcovid[i] { 
         if rand < $pCovid { 
            @hadcovid[i] = True
         } else {
            my $dose = @dosage[i];
            if $dose==0 && rand < $pStartTreatment or $dose > 0 && rand < $pRedose {               
	           @dosage[i]   = $dose += @dRange.roll;
               @category[i] = ($dose > DOSE_FOR_REGULAR) ?? REGULAR !! IRREGULAR
            } 
         } 
      } 
   } 
}

sub kruskal (@sets) {
   my $n          = ( my @ranked = @sets>>.List.flat.sort ).elems;
   my @sr         = 0 xx @sets.elems;
   my $ix         = (@ranked.first: * == True, :k)+1, 
   my ($arf,$art) = ($ix, $ix+$n) >>/>> 2;

   for @sets.kv -> \i,@set { for @set -> $b { @sr[i] += $b ?? $art !! $arf } } 

   my $H = [+] @sr.kv.map: -> \i,\s { s*s/@sets[i].elems }
   return 12/($n*($n+1)) * $H - 3 * ($n + 1)
}

say "Total subjects: $nSubjects\n";

my ($midpoint,$unt,$irr,$reg,$hunt,$hirr,$hreg,@sunt,@sirr,@sreg)=$duration div 2;

for 1 .. $duration -> \day { 
   update();
   if day %% $interval or day == $duration or day == $midpoint { 
      @sunt = @hadcovid[ @category.grep: UNTREATED,:k ];
      @sirr = @hadcovid[ @category.grep: IRREGULAR,:k ]; 
      @sreg = @hadcovid[ @category.grep: REGULAR,  :k ]; 
      ($unt,$hunt,$irr,$hirr,$reg,$hreg)=(@sunt,@sirr,@sreg).map:{|(.elems,.sum)}
   } 
   if day %% $interval {
      printf "Day %d:\n",day;
      printf "Untreated:     N = %4d, with infection = %4d\n",  $unt,$hunt;
      printf "Irregular Use: N = %4d, with infection = %4d\n",  $irr,$hirr;
      printf "Regular Use:   N = %4d, with infection = %4d\n\n",$reg,$hreg
   } 
   if day == $midpoint | $duration { 
      my $stage = day == $midpoint ?? 'midpoint' !! 'study end';
      printf "At $stage, Infection case percentages are:\n";
      printf "  Untreated : %f\n",  100*$hunt/$unt;
      printf "  Irregulars: %f\n",  100*$hirr/$irr;
      printf "  Regulars  : %f\n\n",100*$hreg/$reg
   }    
}

printf "Final statistics: H = %f\n", kruskal ( @sunt, @sirr, @sreg )
```

#### Output:
```
Total subjects: 10000

Day 30:
Untreated:     N = 8616, with infection =  314
Irregular Use: N = 1383, with infection =   27
Regular Use:   N =    1, with infection =    0

Day 60:
Untreated:     N = 7511, with infection =  561
Irregular Use: N = 2308, with infection =   87
Regular Use:   N =  181, with infection =    1

Day 90:
Untreated:     N = 6571, with infection =  760
Irregular Use: N = 2374, with infection =  141
Regular Use:   N = 1055, with infection =   19

At midpoint, Infection case percentages are:
  Untreated : 11.565972
  Irregulars: 5.939343
  Regulars  : 1.800948

Day 120:
Untreated:     N = 5757, with infection =  909
Irregular Use: N = 2097, with infection =  189
Regular Use:   N = 2146, with infection =   61

Day 150:
Untreated:     N = 5092, with infection = 1054
Irregular Use: N = 1832, with infection =  238
Regular Use:   N = 3076, with infection =  132

Day 180:
Untreated:     N = 4530, with infection = 1172
Irregular Use: N = 1615, with infection =  282
Regular Use:   N = 3855, with infection =  229

At study end, Infection case percentages are:
  Untreated : 25.871965
  Irregulars: 17.461300
  Regulars  : 5.940337

Final statistics: H = 248.419454
```
