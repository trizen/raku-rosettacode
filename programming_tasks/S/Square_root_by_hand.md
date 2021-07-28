[1]: https://rosettacode.org/wiki/Square_root_by_hand

# [Square root by hand][1]

Implemented a [long division algorithm.](https://www.wikihow.com/Calculate-a-Square-Root-by-Hand#Finding-Square-Roots-Manually).

```perl
# 20201023 Raku programming solution
 
sub integral   (Str $in) { # prepend '0' if length is odd
   given $in { .chars mod 2 ?? ('0'~$_).comb(2) !! .comb(2) }
}
 
sub fractional (Str $in) { # append  '0' if length is odd
   given $in { .chars mod 2 ?? ($_~'0').comb(2) !! .comb(2) }
}
 
sub SpigotSqrt ($in) {
 
   my @dividends, my @fractional; # holds digital duos
   my $d = 9;    # unit  digit part of divisors & running answer
   my $D = '';   # tens+ digit part of divisors
   my $dot_printed  = False;
   my $dividend; my $quotient = ''; my $remainder;
 
   return "Sorry, minimum charge is $0⁺" if $in ≤ 0;
 
   if $in.narrow ~~ Int {                 # integer
      @dividends = integral($in.Str)
   } else {
      given split(/\./, $in.Str) {        # floating point
         @dividends  =   integral(@_[0]);
         @fractional = fractional(@_[1]);
      }
   }
 
   $dividend = shift @dividends;
 
   loop {
      until ( $remainder = $dividend - ($D~$d) * $d ) ≥ 0 {
         $d-- # keep trying till the max divisor is found
      }
      print $d; # running answer
      $quotient ~= $d;
      unless @dividends.Bool {
         last if ( $remainder == 0 and $quotient != 0 and !@fractional.Bool );
         unless $dot_printed { print '.' and $dot_printed = True }
         if @fractional.Bool {      # happen only once
            @dividends.append: @fractional;
            @fractional = (); # retired
         } else {
            @dividends.append: '00';
         }
      }
      $dividend = $remainder.Str ~ shift @dividends;
      $D = 2*$quotient;
      $d = 9
   }
}
 
#`[ matches result from https://stackoverflow.com/a/28152047/3386748
for <99999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999982920000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000726067> { # ]
for < 25 0.0625 152.2756 13579.02468 > {
   say "The square root of $_ is";
   SpigotSqrt $_ ; print "\n";
}
```

#### Output:
```
The square root of 25 is
5
The square root of 0.0625 is
0.25
The square root of 152.2756 is
12.34
The square root of 13579.02468 is
116.5290722523782903561833846788464631805119729204989141878325473726703822155976113726101636833624692173783050112427274490403132495026916228339453686341013613481116569793281525666303293666139135373395664751766204609166006753350008676787108560810713189340122619853015331030735400702976991920098868235804433621649473896395145322270105611438518020713137788187701241059921153133101219142225340975562189465283743880315403123043908068827985609461380033349440281928044661628680849458194668644072518779930532625670101046028192429778354952392572052578927533919600336446165970115867463651405291843435779882540897283554569528134419570259054368204716277521872340583781499813500950876849873104131526244245476070417915^C
```
