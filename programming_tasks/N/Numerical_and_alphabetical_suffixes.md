[1]: https://rosettacode.org/wiki/Numerical_and_alphabetical_suffixes

# [Numerical and alphabetical suffixes][1]

Scientific notation, while supported in Perl 6, is limited to IEEE-754 64bit accuracy so there is some rounding on values using it. Implements a custom "high precision" conversion routine.



Unfortunately, this suffix routine is of limited use for practical everyday purposes. It focuses on handling excessively large and archaic units (googol, greatgross) and completely ignores or makes unusable (due to forcing case insensitivity) many common current ones: c(centi), m(milli), μ(micro). Ah well.



Note: I am blatantly and deliberately ignoring the task guidelines for formatting the output. It has no bearing on the core of the task. If you really, *really*,* **REALLY** *want to see badly formatted output, uncomment the last line.

```perl
use Rat::Precise;
 
my $googol = 10**100;
«PAIRs 2 SCOres 20 DOZens 12 GRoss 144  GREATGRoss 1728 GOOGOLs $googol»
  ~~ m:g/ ((<.:Lu>+) <.:Ll>*) \s+ (\S+) /;
 
my %abr = |$/.map: {
    my $abbrv = .[0].Str.fc;
    my $mag   = +.[1];
    |map { $abbrv.substr( 0, $_ ) => $mag },
    .[0][0].Str.chars .. $abbrv.chars
}
 
my %suffix = flat %abr,
(<K  M  G  T  P  E  Z  Y  X  W  V  U>».fc  Z=> (1000, * * 1000 … *)),
(<Ki Mi Gi Ti Pi Ei Zi Yi Xi Wi Vi Ui>».fc Z=> (1024, * * 1024 … *));
 
my $reg = %suffix.keys.join('|');
 
sub comma ($i is copy) {
    my $s = $i < 0 ?? '-' !! '';
    my ($whole, $frac) = $i.split('.');
    $frac = $frac.defined ?? ".$frac" !! '';
    $s ~ $whole.abs.flip.comb(3).join(',').flip ~ $frac
}
 
sub units (Str $str) {
    $str.fc ~~ /^(.+?)(<alpha>*)('!'*)$/;
    my ($val, $unit, $fact) = $0, $1.Str.fc, $2.Str;
    $val.=subst(',', '', :g);
    if $val ~~ m:i/'e'/ {
        my ($m,$e) = $val.split(/<[eE]>/);
        $val = ($e < 0)
            ?? $m * FatRat.new(1,10**-$e)
            !! $m * 10**$e;
    }
    my @suf = $unit;
    unless %suffix{$unit}:exists {
        $unit ~~ /(<$reg>)+/;
        @suf = $0;
    }
    my $ret = $val<>;
    $ret = [*] $ret, |@suf.map: { %suffix{$_} } if @suf[0];
    $ret = [*] ($ret, * - $fact.chars …^ * < 2) if $fact.chars;
    $ret.?precise // $ret
}
 
my $test = q:to '===';
2greatGRo   24Gros  288Doz  1,728pairs  172.8SCOre
1,567      +1.567k    0.1567e-2m
25.123kK    25.123m   2.5123e-00002G
25.123kiKI  25.123Mi  2.5123e-00002Gi  +.25123E-7Ei
-.25123e-34Vikki      2e-77gooGols
9!   9!!   9!!!   9!!!!   9!!!!!   9!!!!!!   9!!!!!!!   9!!!!!!!!   9!!!!!!!!!
.017k!!
===
 
printf "%16s: %s\n", $_, comma .&units for $test.words;
 
# Task required stupid layout
# say "\n In: $_\nOut: ", .words.map({comma .&units}).join('  ') for $test.lines;
```

#### Output:
```
       2greatGRo: 3,456
          24Gros: 3,456
          288Doz: 3,456
      1,728pairs: 3,456
      172.8SCOre: 3,456
           1,567: 1,567
         +1.567k: 1,567
      0.1567e-2m: 1,567
        25.123kK: 25,123,000
         25.123m: 25,123,000
  2.5123e-00002G: 25,123,000
      25.123kiKI: 26,343,374.848
        25.123Mi: 26,343,374.848
 2.5123e-00002Gi: 26,975,615.844352
    +.25123E-7Ei: 28,964,846,960.237816578048
-.25123e-34Vikki: -33,394.194938104441474962344775423096782848
    2e-77gooGols: 200,000,000,000,000,000,000,000
              9!: 362,880
             9!!: 945
            9!!!: 162
           9!!!!: 45
          9!!!!!: 36
         9!!!!!!: 27
        9!!!!!!!: 18
       9!!!!!!!!: 9
      9!!!!!!!!!: 9
         .017k!!: 34,459,425
```