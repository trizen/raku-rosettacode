[1]: https://rosettacode.org/wiki/Continued_fraction_convergents

# [Continued fraction convergents][1]

```perl
# 20240210 Raku programming solution

sub convergents(Real $x is copy, Int $maxcount) {
   my @components = gather for ^$maxcount {
      my $fpart = $x - take $x.Int;
      $fpart == 0 ?? ( last ) !! ( $x = 1 / $fpart )
   }
   my ($numa, $denoma, $numb, $denomb) = 1, 0, @components[0], 1;
   return [ Rat.new($numb, $denomb) ].append: gather for @components[1..*] -> $comp {
      ( $numa, $denoma, $numb                , $denomb                  ) 
      = $numb, $denomb, $numa + $comp * $numb, $denoma + $comp * $denomb;
      take Rat.new($numb, $denomb);
   }
}

my @tests = [ "415/93", 415/93, "649/200", 649/200, "sqrt(2)", sqrt(2),
              "sqrt(5)", sqrt(5), "golden ratio", (sqrt(5) + 1) / 2     ];

say "The continued fraction convergents for the following (maximum 8 terms) are:";
for @tests -> $s, $x {
   say $s.fmt('%15s') ~ " = { convergents($x, 8).map: *.nude.join('/') } ";
}
```

#### Output:
```
The continued fraction convergents for the following (maximum 8 terms) are:
         415/93 = 4/1 9/2 58/13 415/93 
        649/200 = 3/1 13/4 159/49 649/200 
        sqrt(2) = 1/1 3/2 7/5 17/12 41/29 99/70 239/169 577/408 
        sqrt(5) = 2/1 9/4 38/17 161/72 682/305 2889/1292 12238/5473 51841/23184 
   golden ratio = 1/1 2/1 3/2 5/3 8/5 13/8 21/13 34/21 
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=dVNNb9pAED305l_xYrliTRdj01AREE2vvUa5ISotsCZu7LW7u25AEfkjveTQ_qn8mo6_EKDWF8_OvDezM2_n128tHsvX1z-ljQeTt3fClCusc_VT6q1U1rA7KVJ4OySG3MWe46uy8DKxW-elsj6eHQDZHl_WeVbkquJgjq2wD1IjzjW-HcENtoF7cSG0JSSlHsCKR0lWQLlnLaYDzBHi9hYMqTAWPq6uyCbSHBGGHcqvSIf2JsxTZSY4vI1UeW3QedWdV35F5Qj56ZUX4ZIjqmtraUutsMCdsIGST-ySvgxEUUi1mZ52eZorCoL-EoPP8CrnsWuGf14MF9-x0mUA1H2ban7RU5NK4ENbsn8ePws0jG7M9eT_0-msmerBcSp9rTS1tAu419F4ePPR5WgMDvfT9c1wFIbkai3ymR_aspFPvtbiznk7DWJ8RIx9Ym3zlMpDC5vkFGBthDqIfBJ8VDOXM8cxYg_3_kFWr9UmqpQbxFqsiaZOH3CtDslE_zTNnxK1BaMHmWRlhgms1JnxIbScujOnFrLps1LP8Oqh1fJVxTwTxJllvffR2PR8vMClaTyfLYu345j4QSaKKfqBKjcy-J4nivWGRDiAShycZtPahesW7y8)
