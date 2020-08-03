[1]: https://rosettacode.org/wiki/Negative_base_numbers

# [Negative base numbers][1]

Perl 6 provides built-in methods / routines base and parse-base to convert to and from bases 2 through 36. We'll just shadow the core routines with versions that accept negative bases.



As a stretch goal, rather than implement something that can "Spell the name of the language with correct capitalization" (which is silly, trivial, and has absolutely ***nothing*** to do with negative base numbers,) we'll implement routines that correctly work with any Real number, not just integers.



The Real candidate has a 'precision' parameter, default -15, (15 places after the decimal point,) to limit the length of the fractional portion of the converted value. Change it if you need or want different precision. The Integer only routine is kept here as a multi-dispatch candidate since it is potentially much faster than the Real capable routine. The dispatcher will automatically use the most appropriate (fastest) routine.



Note that the parse-base routine will handle 'illegal' negative negative-base values without blowing up.

```raku
multi sub base ( Int $value is copy, Int $radix where -37 < * < -1) {
    my $result;
    while $value {
        my $r = $value mod $radix;
        $value div= $radix;
        if $r < 0 {
            $value++;
            $r -= $radix
        }
        $result ~= $r.base(-$radix);
    }
    flip $result || ~0;
}
 
multi sub base ( Real $num, Int $radix where -37 < * < -1, :$precision = -15 ) {
    return '0' unless $num;
    my $value  = $num;
    my $result = '';
    my $place  = 0;
    my $upper-bound = 1 / (-$radix + 1);
    my $lower-bound = $radix * $upper-bound;
 
    $value = $num / $radix ** ++$place until $lower-bound <= $value < $upper-bound;
 
    while ($value or $place > 0) and $place > $precision {
        my $digit = ($radix * $value - $lower-bound).Int;
        $value    =  $radix * $value - $digit;
        $result ~= '.' unless $place or $result.contains: '.';
        $result ~= $digit == -$radix ?? ($digit-1).base(-$radix)~'0' !! $digit.base(-$radix);
        $place--
    }
    $result
}
 
multi sub parse-base (Str $str, Int $radix where -37 < * < -1) {
    return -1 * $str.substr(1).&parse-base($radix) if $str.substr(0,1) eq '-';
    my ($whole, $frac) = $str.split: '.';
    my $fraction = 0;
    $fraction = [+] $frac.comb.kv.map: { $^v.parse-base(-$radix) * $radix ** -($^k+1) } if $frac;
    $fraction + [+] $whole.flip.comb.kv.map: { $^v.parse-base(-$radix) * $radix ** $^k }
}
 
# TESTING
for <4 -4 0 -7  10 -2  146 -3  15 -10  -19 -10  107 -16
    227.65625 -16  2.375 -4 -1.3e2 -8 41371457.268272761 -36> -> $v, $r {
    my $nbase = $v.&base($r, :precision(-5));
    printf "%20s.&base\(%3d\) = %-11s : %13s.&parse-base\(%3d\) = %s\n",
      $v, $r, $nbase, "'$nbase'", $r, $nbase.&parse-base($r);
}
 
# 'Illegal' negative-base value
say q|  '-21'.&parse-base(-10) = |, '-21'.&parse-base(-10);
```

#### Output:
```
                   4.&base( -4) = 130         :         '130'.&parse-base( -4) = 4
                   0.&base( -7) = 0           :           '0'.&parse-base( -7) = 0
                  10.&base( -2) = 11110       :       '11110'.&parse-base( -2) = 10
                 146.&base( -3) = 21102       :       '21102'.&parse-base( -3) = 146
                  15.&base(-10) = 195         :         '195'.&parse-base(-10) = 15
                 -19.&base(-10) = 21          :          '21'.&parse-base(-10) = -19
                 107.&base(-16) = 1AB         :         '1AB'.&parse-base(-16) = 107
           227.65625.&base(-16) = 124.68      :      '124.68'.&parse-base(-16) = 227.65625
               2.375.&base( -4) = 3.32        :        '3.32'.&parse-base( -4) = 2.375
              -1.3e2.&base( -8) = 1616        :        '1616'.&parse-base( -8) = -130
  41371457.268272761.&base(-36) = PERL6.ROCKS : 'PERL6.ROCKS'.&parse-base(-36) = 41371457.268272761
  '-21'.&parse-base(-10) = 19
```