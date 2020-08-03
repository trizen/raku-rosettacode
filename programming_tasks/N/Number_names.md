[1]: https://rosettacode.org/wiki/Number_names

# [Number names][1]

Apart from the `$m++` this can be viewed as a purely functional program; we use nested `gather`/`take` constructs to avoid accumulators.

```raku
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


Alternately, we could use the Lingua::EN::Numbers::Cardinal module from the Perl 6 ecosystem. It will return similar output for similar inputs as above, but also handles fractions with configurable reduction and denominator, exponential notation, and ordinal notation.

```raku
use Lingua::EN::Numbers::Cardinal;
 
put join "\n", .&cardinal, .&cardinal(:improper) with -7/4;
 
printf "%-7s : %19s : %s\n", $_, cardinal($_), cardinal($_, :denominator(16)) for 1/16, 2/16 ... 1;
 
put join "\n", .&cardinal, .&cardinal-year, .&ordinal, .&ordinal-digit with 1999;
 
.&cardinal.put for 6.022e23, 42000, π;
```

#### Output:
```
negative one and three quarters
negative seven quarters
0.0625  :       one sixteenth : one sixteenth
0.125   :          one eighth : two sixteenths
0.1875  :    three sixteenths : three sixteenths
0.25    :         one quarter : four sixteenths
0.3125  :     five sixteenths : five sixteenths
0.375   :       three eighths : six sixteenths
0.4375  :    seven sixteenths : seven sixteenths
0.5     :            one half : eight sixteenths
0.5625  :     nine sixteenths : nine sixteenths
0.625   :        five eighths : ten sixteenths
0.6875  :   eleven sixteenths : eleven sixteenths
0.75    :      three quarters : twelve sixteenths
0.8125  : thirteen sixteenths : thirteen sixteenths
0.875   :       seven eighths : fourteen sixteenths
0.9375  :  fifteen sixteenths : fifteen sixteenths
1       :                 one : one
one thousand, nine hundred ninety-nine
nineteen ninety-nine
one thousand, nine hundred ninety-ninth
1999th
six point zero two two times ten to the twenty-third
forty-two thousand
three point one four one five nine two six five three five eight nine seven nine
```