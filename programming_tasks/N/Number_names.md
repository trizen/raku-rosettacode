[1]: http://rosettacode.org/wiki/Number_names

# [Number names][1]

Apart from the <tt>$m++</tt> this can be viewed as a purely functional program; we use nested <tt>gather</tt>/<tt>take</tt> constructs to avoid accumulators.

```perl6
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
 
while '' ne (my $n = prompt("Number: ")) {
    say int-name($n);
}
```


Output:


#### Output:
```
Number: 0
zero
Number: 17
seventeen
Number: -1,234,567,890          
negative one billion, two hundred thirty four million, five hundred sixty seven thousand, eight hundred ninety
Number: 42 000
forty two thousand
Number: 1001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001001
one novemseptuagintillion, one octoseptuagintillion, one septenseptuagintillion, one sexseptuagintillion, one quinseptuagintillion, one quattuorseptuagintillion, one treseptuagintillion, one duoseptuagintillion, one unseptuagintillion, one septuagintillion, one novemsexagintillion, one octosexagintillion, one septensexagintillion, one sexsexagintillion, one quinsexagintillion, one quattuorsexagintillion, one tresexagintillion, one duosexagintillion, one unsexagintillion, one sexagintillion, one novemquinquagintillion, one octoquinquagintillion, one septenquinquagintillion, one sexquinquagintillion, one quinquinquagintillion, one quattuorquinquagintillion, one trequinquagintillion, one duoquinquagintillion, one unquinquagintillion, one quinquagintillion, one novemquadragintillion, one octoquadragintillion, one septenquadragintillion, one sexquadragintillion, one quinquadragintillion, one quattuorquadragintillion, one trequadragintillion, one duoquadragintillion, one unquadragintillion, one quadragintillion, one novemtrigintillion, one octotrigintillion, one septentrigintillion, one sextrigintillion, one quintrigintillion, one quattuortrigintillion, one tretrigintillion, one duotrigintillion, one untrigintillion, one trigintillion, one novemvigintillion, one octovigintillion, one septenvigintillion, one sexvigintillion, one quinvigintillion, one quattuorvigintillion, one trevigintillion, one duovigintillion, one unvigintillion, one vigintillion, one novemdecillion, one octodecillion, one septendecillion, one sexdecillion, one quindecillion, one quattuordecillion, one tredecillion, one duodecillion, one undecillion, one decillion, one nonillion, one octillion, one septillion, one sextillion, one quintillion, one quadrillion, one trillion, one billion, one million, one thousand, one
Number: 198723483017417
one hundred ninety eight trillion, seven hundred twenty three billion, four hundred eighty three million, seventeen thousand, four hundred seventeen
```