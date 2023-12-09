[1]: https://rosettacode.org/wiki/Numbers_k_such_that_the_last_letter_of_k_is_the_same_as_the_first_letter_of_k%2B1

# [Numbers k such that the last letter of k is the same as the first letter of k+1][1]

```perl
# 20230713 Raku programming solution

###### https://rosettacode.org/wiki/Number_names#Raku

constant @I = <zero one    two    three    four     five    six     seven     eight    nine
               ten  eleven twelve thirteen fourteen fifteen sixteen seventeen eighteen nineteen>;
constant @X = <0    X      twenty thirty   forty    fifty   sixty   seventy   eighty   ninety>;
constant @C = @I X~ ' hundred';
constant @M = (<0 thousand>,
    ((<m b tr quadr quint sext sept oct non>,
    (map { ('', <un duo tre quattuor quin sex septen octo novem>).flat X~ $_ },
    <dec vigint trigint quadragint quinquagint sexagint septuagint octogint nonagint>),
    'cent').flat X~ 'illion')).flat;

sub int-name ($num) {
    if $num.substr(0,1) eq '-' { return "negative {int-name($num.substr(1))}" }
    if $num eq '0' { return @I[0] }
    my $m = 0;
    return join ', ', reverse gather for $num.flip.comb(/\d ** 1..3/) {
        my ($i,$x,$c) = .comb».Int;
        if $i or $x or $c {
            take join ' ', gather {
                if $c { take @C[$c] }
                if $x and $x == 1 { take @I[$i+10] }
                else {
                    if $x { take @X[$x] }
                    if $i { take @I[$i] }
                }
                take @M[$m] // die "WOW! ZILLIONS!\n" if $m;
            }
        }
        $m++;
    }
}
######

my ($i, $c, $limit, $prev, @nums, @lastDigs) = 0, 0, 1000, int-name(0);

while $limit <= 1e4 {
   my $next = int-name $i+1;
   if $prev.substr(*-1) eq $next.substr(0,1) {
      if ($c < 50) { @nums.append: $i };
      @lastDigs[$i % 10] += 1;
      $c++;
      if $c == 50 {
         say "First 50 numbers:";
         say [~] $_>>.fmt('%4s') for @nums.rotor(10);
         say(); 
      } elsif $c == $limit {
         print "The {$c}th number is $i.\n";
         say "Breakdown by last digit of first {$c}th numbers";
         say 'N Freq';
         for 0..9 -> $d { 
            say "$d {@lastDigs[$d].fmt('%4s')} ",
                '█' x (@lastDigs[$d]/@lastDigs.max*72).Int;
         }
         say(); 
         $limit *= 10
      }
   }
   $prev = $next;
   $i++;
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=bVbBbttGEL0W_IoJw4CkLFFUm6CtJQtGUwQQkDhAWyBGFSOgyJW0tbgrL5eWVIM59wtyySWH9it662f02C_pzC4pUbYJmTu7nHnzduYtzc9_quS6_PLlr1LPe9_9-5X71Fyw1HpdnPb7ShZM6ySVGYukWvQ3_Jr3L8p8xtQHkeSsePoTxjtOKkWhE6HhfAJnMPqdKQlSMMBLb6QZloqZ-VyWCozBb81CwbdmXrBbJozF-GKpyRBcMAeOL01ObGWc9YatEEQvudIM54RtDT43I2LbkdyNZbDJIGwyxsMW-0tiH1OayzrbBuN2NsPOsLejybCz7O1oMuwa9ruavd4d4b9EfCzR5UfwYVmKTLHMbz9_g88DJKCXsiwSkY27ZvtBMMphBlrBTZlkdOfoXbAt3dYaZKpBSNF458ka7iDw_S6MSgFZKTGUUazWpbThFG2CsRQYLjH-luXjMJqvEk0EvQ9QWbxRxlK45QvKqZUdDY-kNrnA6aKm1BhrXa8RujGQoVkZhxbXT7Fi_iGjz1crLoUf2qWh4xTlDDCgR1KDwBNlHsKdieVzoGmEHoVWQdwdhMBuwO_5uHPFdKkEuIItEk0qu2tAgnbQIAwrF6o2nsGIWxjnk2l8VfvkO_BybFA8NNPa4zeJxcRK40-hBlTBALMumSKtWJLzFV9HqcxnQf99Bp0ODKLom36zlRo68HjX23a9NMQUxvufv6OJ0MO9E3HkQKBbc09bAEaryTWr6RCbmsXd_QNkcDDW-p-_nHpps8P7XltADdJwdgaDfcRk6vGTQfxYEFvh9h9mPOA1GJdTb_sYwGGf7WyPeT5csf5vpl5-Bf0-ZJyB--7tuyfw6-T168nbi5-fvBeuQc-HzuNQB8vLT06sV-VUjn0tOk7dJ6wf_q14zjWOa-x7F86x0wUOq6TQP_JFQW2Mu_QbxDHe9xKMQ1T2ZslXrIaAEVaXPbd1I5EJOthnB-VTuQ0X4k7ZGgV3elb2JuLoLDQ9wIgAmz2CFzEuWpJRsl4zkZ1SlaumEnveWG54BtTeE6TVPPbSphyNflASL-J2r4tkB-4rrgpND4T5J1GcusNjj-nHK3y1jMfRPNeB_-x54YfmoFhmSmqJB5NK1I4KwiHUCxVpbM-gLmCLxVrRq8b9ZYk69NJKL2smwAvcb4QKuEfI_UGx5DqTGwGzHVAVUDkLBJVzfMnTbo5wivvx_gW8UuzGby3TfuIo-h56Y_AyLPuR2kxSWm6VPLtq1aMCt_tA3P5_n_7wYQvBUVR_P4vyZNv59uvw-JXRPiXHdaSe2uJ1sM2x03I3N6MzFKGRlgFEFaIEKvupUH8xNF8O_wM)
