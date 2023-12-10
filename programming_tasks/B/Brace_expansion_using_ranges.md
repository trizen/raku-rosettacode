[1]: https://rosettacode.org/wiki/Brace_expansion_using_ranges

# [Brace expansion using ranges][1]

Also implements some of the string list functions described on the bash-hackers page.

```perl
my $range = rx/ '{' $<start> = <-[.]>+? '..' $<end> = <-[.]>+? ['..' $<incr> = ['-'?\d+] ]? '}' /;
my $list  = rx/ ^ $<prefix> = .*? '{' (<-[,}]>+) +%% ',' '}' $<postfix> = .* $/;

sub expand (Str $string) {
    my @return = $string;
    if $string ~~ $range {
        quietly my ($start, $end, $incr) = $/<start end incr>».Str;
        $incr ||= 1;
        ($end, $start) = $start, $end if $incr < 0;
        $incr.=abs;

        if try all( +$start, +$end ) ~~ Numeric {
            $incr = - $incr if $start > $end;

            my ($sl, $el) = 0, 0;
            $sl = $start.chars if $start.starts-with('0');
            $el = $end.chars   if   $end.starts-with('0');

            my @this = $start < $end ?? (+$start, * + $incr …^ * > +$end) !! (+$start, * + $incr …^ * < +$end);
            @return  = @this.map: { $string.subst($range, sprintf("%{'0' ~ max $sl, $el}d", $_) ) }
        }
        elsif try +$start ~~ Numeric or +$end ~~ Numeric {
            return $string #fail
        }
        else {
            my @this;
            if $start.chars + $end.chars > 2 {
                return $string if $start.succ eq $start or $end.succ eq $end; # fail
                @this = $start lt $end ?? ($start, (*.succ xx $incr).tail …^ * gt $end) !! ($start, (*.pred xx $incr).tail …^ * lt $end);
            }
            else {
                $incr = -$incr if $start gt $end;
                @this = $start lt $end ?? ($start, (*.ord + $incr).chr …^ * gt $end) !! ($start, (*.ord + $incr).chr …^ * lt $end);
            }
            @return = @this.map: { $string.subst($range, sprintf("%s", $_) ) }
        }
    }
    if $string ~~ $list {
        my $these = $/[0]».Str;
        my ($prefix, $postfix) = $/<prefix postfix>».Str;
        if ($prefix ~ $postfix).chars {
            @return = $these.map: { $string.subst($list, $prefix ~ $_ ~ $postfix) } if $these.elems > 1
        }
        else {
            @return = $these.join: ' '
        }
    }
    my $cnt = 1;
    while $cnt != +@return {
        $cnt = +@return;
        @return.=map: { |.&expand }
    }
    @return
}

for qww<
    # Required tests

    simpleNumberRising{1..3}.txt
    simpleAlphaDescending-{Z..X}.txt
    steppedDownAndPadded-{10..00..5}.txt
    minusSignFlipsSequence{030..20..-5}.txt
    combined-{Q..P}{2..1}.txt
    emoji{🌵..🌶}{🌽..🌾}etc
    li{teral
    rangeless{}empty
    rangeless{random}string

    # Test some other features

    'stop point not in sequence-{02..10..3}.txt'
    steppedAlphaRising{P..Z..2}.txt
    'simple {just,give,me,money} list'
    {thatʼs,what,I,want}
    'emoji {☃,☄}{★,🇺🇸,☆} lists'
    'alphanumeric mix{ab7..ac1}.txt'
    'alphanumeric mix{0A..0C}.txt'

    # fail by design

    'mixed terms fail {7..C}.txt'
    'multi char emoji ranges fail {🌵🌵..🌵🌶}'
  > -> $test {
     say "$test ->";
     say ('    ' xx * Z~ expand $test).join: "\n";
     say '';
}
```

#### Output:
```
simpleNumberRising{1..3}.txt ->
    simpleNumberRising1.txt
    simpleNumberRising2.txt
    simpleNumberRising3.txt

simpleAlphaDescending-{Z..X}.txt ->
    simpleAlphaDescending-Z.txt
    simpleAlphaDescending-Y.txt
    simpleAlphaDescending-X.txt

steppedDownAndPadded-{10..00..5}.txt ->
    steppedDownAndPadded-10.txt
    steppedDownAndPadded-05.txt
    steppedDownAndPadded-00.txt

minusSignFlipsSequence{030..20..-5}.txt ->
    minusSignFlipsSequence020.txt
    minusSignFlipsSequence025.txt
    minusSignFlipsSequence030.txt

combined-{Q..P}{2..1}.txt ->
    combined-Q2.txt
    combined-Q1.txt
    combined-P2.txt
    combined-P1.txt

emoji{🌵..🌶}{🌽..🌾}etc ->
    emoji🌵🌽etc
    emoji🌵🌾etc
    emoji🌶🌽etc
    emoji🌶🌾etc

li{teral ->
    li{teral

rangeless{}empty ->
    rangeless{}empty

rangeless{random}string ->
    rangeless{random}string

stop point not in sequence-{02..10..3}.txt ->
    stop point not in sequence-02.txt
    stop point not in sequence-05.txt
    stop point not in sequence-08.txt

steppedAlphaRising{P..Z..2}.txt ->
    steppedAlphaRisingP.txt
    steppedAlphaRisingR.txt
    steppedAlphaRisingT.txt
    steppedAlphaRisingV.txt
    steppedAlphaRisingX.txt
    steppedAlphaRisingZ.txt

simple {just,give,me,money} list ->
    simple just list
    simple give list
    simple me list
    simple money list

{thatʼs,what,I,want} ->
    thatʼs what I want

emoji {☃,☄}{★,🇺🇸,☆} lists ->
    emoji ☃★ lists
    emoji ☃🇺🇸 lists
    emoji ☃☆ lists
    emoji ☄★ lists
    emoji ☄🇺🇸 lists
    emoji ☄☆ lists

alphanumeric mix{ab7..ac1}.txt ->
    alphanumeric mixab7.txt
    alphanumeric mixab8.txt
    alphanumeric mixab9.txt
    alphanumeric mixac0.txt
    alphanumeric mixac1.txt

alphanumeric mix{0A..0C}.txt ->
    alphanumeric mix0A.txt
    alphanumeric mix0B.txt
    alphanumeric mix0C.txt

mixed terms fail {7..C}.txt ->
    mixed terms fail {7..C}.txt

multi char emoji ranges fail {🌵🌵..🌵🌶} ->
    multi char emoji ranges fail {🌵🌵..🌵🌶}
```
