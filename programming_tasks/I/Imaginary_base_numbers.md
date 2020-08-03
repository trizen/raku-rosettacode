[1]: https://rosettacode.org/wiki/Imaginary_base_numbers

# [Imaginary base numbers][1]

These are generalized imaginary-base conversion routines. They only work for imaginary bases, not complex. (Any real portion of the radix must be zero.) Theoretically they could be made to work for any imaginary base; in practice, they are limited to integer bases from -6i to -2i and 2i to 6i. Bases -1i and 1i exist but require special handling and are not supported. Bases larger than 6i (or -6i) require digits outside of base 36 to express them, so aren't as standardized, are implementation dependent and are not supported. Note that imaginary number coefficients are stored as floating point numbers in Perl 6 so some rounding error may creep in during calculations. The precision these conversion routines use is configurable; we are using 6 <strike>decimal</strike>, um... radicimal(?) places of precision here.



Implements minimum, extra kudos and stretch goals.

```raku
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
 
multi sub base (Numeric $num, Complex $radix where *.re == 0, :$precision = -8 ) {
    die "Base $radix out of range" unless -6 <= $radix.im <= -2 or 2 <= $radix.im <= 6;
    my ($re, $im) = $num.Complex.reals;
    my ($re-wh, $re-fr) =             $re.&base( -$radix.im².Int, :precision($precision) ).split: '.';
    my ($im-wh, $im-fr) = ($im/$radix.im).&base( -$radix.im².Int, :precision($precision) ).split: '.';
    $_ //= '' for $re-fr, $im-fr;
 
    sub zip (Str $a, Str $b) {
        my $l = '0' x ($a.chars - $b.chars).abs;
        ([~] flat ($a~$l).comb Z flat ($b~$l).comb).subst(/ '0'+ $ /, '') || '0'
    }
 
    my $whole = flip zip $re-wh.flip, $im-wh.flip;
    my $fraction = zip $im-fr, $re-fr;
    $fraction eq 0 ?? "$whole" !! "$whole.$fraction"
}
 
multi sub parse-base (Str $str, Complex $radix where *.re == 0) {
    return -1 * $str.substr(1).&parse-base($radix) if $str.substr(0,1) eq '-';
    my ($whole, $frac) = $str.split: '.';
    my $fraction = 0;
    $fraction = [+] $frac.comb.kv.map: { $^v.parse-base($radix.im².Int) * $radix ** -($^k+1) } if $frac;
    $fraction + [+] $whole.flip.comb.kv.map: { $^v.parse-base($radix.im².Int) * $radix ** $^k }
}
 
# TESTING
for 0, 2i, 1, 2i, 5, 2i, -13, 2i, 9i, 2i, -3i, 2i, 7.75-7.5i, 2i, .25, 2i, # base 2i tests
    5+5i,  2i, 5+5i,  3i, 5+5i,  4i, 5+5i,  5i, 5+5i,  6i, # same value, positive imaginary bases
    5+5i, -2i, 5+5i, -3i, 5+5i, -4i, 5+5i, -5i, 5+5i, -6i, # same value, negative imaginary bases
    227.65625+10.859375i, 4i, # larger test value
    31433.3487654321-2902.4480452675i, 6i # heh
  -> $v, $r {
my $ibase = $v.&base($r, :precision(-6));
printf "%33s.&base\(%2si\) = %-11s : %13s.&parse-base\(%2si\) = %s\n",
  $v, $r.im, $ibase, "'$ibase'", $r.im, $ibase.&parse-base($r).round(1e-10).narrow;
}
```

#### Output:
```
                                0.&base( 2i) = 0           :           '0'.&parse-base( 2i) = 0
                                1.&base( 2i) = 1           :           '1'.&parse-base( 2i) = 1
                                5.&base( 2i) = 10301       :       '10301'.&parse-base( 2i) = 5
                              -13.&base( 2i) = 1030003     :     '1030003'.&parse-base( 2i) = -13
                             0+9i.&base( 2i) = 103010.2    :    '103010.2'.&parse-base( 2i) = 0+9i
                            -0-3i.&base( 2i) = 1030.2      :      '1030.2'.&parse-base( 2i) = 0-3i
                        7.75-7.5i.&base( 2i) = 11210.31    :    '11210.31'.&parse-base( 2i) = 7.75-7.5i
                             0.25.&base( 2i) = 1.03        :        '1.03'.&parse-base( 2i) = 0.25
                             5+5i.&base( 2i) = 10331.2     :     '10331.2'.&parse-base( 2i) = 5+5i
                             5+5i.&base( 3i) = 25.3        :        '25.3'.&parse-base( 3i) = 5+5i
                             5+5i.&base( 4i) = 25.C        :        '25.C'.&parse-base( 4i) = 5+5i
                             5+5i.&base( 5i) = 15          :          '15'.&parse-base( 5i) = 5+5i
                             5+5i.&base( 6i) = 15.6        :        '15.6'.&parse-base( 6i) = 5+5i
                             5+5i.&base(-2i) = 11321.2     :     '11321.2'.&parse-base(-2i) = 5+5i
                             5+5i.&base(-3i) = 1085.6      :      '1085.6'.&parse-base(-3i) = 5+5i
                             5+5i.&base(-4i) = 10F5.4      :      '10F5.4'.&parse-base(-4i) = 5+5i
                             5+5i.&base(-5i) = 10O5        :        '10O5'.&parse-base(-5i) = 5+5i
                             5+5i.&base(-6i) = 5.U         :         '5.U'.&parse-base(-6i) = 5+5i
             227.65625+10.859375i.&base( 4i) = 10234.5678  :  '10234.5678'.&parse-base( 4i) = 227.65625+10.859375i
31433.3487654321-2902.4480452675i.&base( 6i) = PERL6.ROCKS : 'PERL6.ROCKS'.&parse-base( 6i) = 31433.3487654321-2902.4480452675i
```