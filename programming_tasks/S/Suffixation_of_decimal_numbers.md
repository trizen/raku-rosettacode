[1]: https://rosettacode.org/wiki/Suffixation_of_decimal_numbers

# [Suffixation of decimal numbers][1]





Pass in a number string, optionally a type, and optionally the number of digits to round to.



The types supported are B, M &amp; G for binary, metric or gigantic. (At this point, the only gigantic unit is googol, so maybe it stands for googol. ¯\\_(ツ)\_/¯ )



If no type is specified, M (metric) is assumed.



If you desire the number to be rounded, pass in a number representing the placed past the decimal to round to. If you pass in a negative number for rounding, it will round to a negative number of places past the decimal.

```perl
sub sufficate ($val is copy, $type is copy = 'M', $round is copy = Any) {
   if +$type ~~ Int { $round = $type; $type = 'M' }
   my $s = '';
   if $val.substr(0,1) eq '-' { $s = '-'; $val.=substr(1) }
   $val.=subst(',', '', :g);
   if $val ~~ m:i/'e'/ {
       my ($m,$e) = $val.split(/<[eE]>/);
       $val = ($e < 0)
           ?? $m * FatRat.new(1,10**-$e)
           !! $m * 10**$e;
   }
   my %s = do given $type {
       when 'M' { <K M G T P E Z Y X W V U> Z=> (1000, * * 1000 … *) }
       when 'B' { <Ki Mi Gi Ti Pi Ei Zi Yi Xi Wi Vi Ui> Z=> (1024, * * 1024 … *) }
       when 'G' { googol => 10**100 }
       default { return 'What we have here is a failure to communicate...' }
   }
   my $k = do given $val {
       when .abs < (my $m = min %s.values) { %s.first( *.value == $m ).key };
       when .abs > (my $x = max %s.values) { %s.first( *.value == $x ).key };
       default { %s.sort(*.value).first({$val.abs/%s{$_.key} < min %s.values}).key}
   }
   $round.defined
       ?? $s ~ comma(($val.abs/%s{$k}).round(10**-$round)) ~ $k
       !! $s ~ comma($val.abs/%s{$k}) ~ $k
}

sub comma ($i is copy) {
    my $s = $i < 0 ?? '-' !! '';
    my ($whole, $frac) = $i.split('.');
    $frac = $frac.defined ?? ".$frac" !! '';
    $s ~ $whole.abs.flip.comb(3).join(',').flip ~ $frac
}

## TESTING

my @tests =
   '87,654,321',
   '-998,877,665,544,332,211,000 3',
   '+112,233 0',
   '16,777,216 1',
   '456,789,100,000,000',
   '456,789,100,000,000 M 2',
   '456,789,100,000,000 B 5',
   '456,789,100,000.000e+00 M 0',
   '+16777216 B',
   '1.2e101 G',
   "{run('df', '/', :out).out.slurp.words[10] * 1024} B 2", # Linux df returns Kilobytes by default
   '347,344 M -2', # round to -2 past the decimal
   '1122334455 Q', # bad unit type example
;

printf "%33s : %s\n", $_, sufficate(|.words) for @tests;
```

#### Output:
```
                       87,654,321 : 87.654321M
   -998,877,665,544,332,211,000 3 : -998.878E
                       +112,233 0 : 112K
                     16,777,216 1 : 16.8M
              456,789,100,000,000 : 456.7891T
          456,789,100,000,000 M 2 : 456.79T
          456,789,100,000,000 B 5 : 415.44727Ti
      456,789,100,000.000e+00 M 0 : 457G
                      +16777216 B : 16Mi
                        1.2e101 G : 12googol
                 703674818560 B 2 : 655.35Gi
                     347,344 M -2 : 300K
                     1122334455 Q : What we have here is a failure to communicate...
```
