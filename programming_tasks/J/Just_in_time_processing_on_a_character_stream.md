[1]: https://rosettacode.org/wiki/Just_in_time_processing_on_a_character_stream

# [Just in time processing on a character stream][1]

This is a somewhat fragile toy encoder / decoder and shouldn't be used for anything serious.

```perl
#`[
Set srand to set the encode / decode "key", and
generate a lazy, reproducible stream of "random"
characters. Need to use the same key and same
implementation of Perl 6 to encode / decode.
Gain "security" by exchanging keys by a second
channel. Default key is 'Perl 6'
]
 
unit sub MAIN ($key = 'Perl 6');
 
srand $key.comb(/<.alnum>/).join.parse-base(36) % 2**63;
 
# random but reproducible infinite stream of characters
my @stream = (flat ' ' .. '~').roll(*);
 
sub jit-encode (Str $str) {
    my $i = 0;
    my $last = 0;
    my $enc = '';
    for $str.comb -> $c {
        my $h;
        my $l = '';
        ++$i until $c eq @stream[$i];
        my $o = $i - $last;
        $l    = $o % 26;
        $h    = $o - $l if $o > 26;
        $l   += 10;
        $enc ~= ($h ?? $h.base(36).uc !! '') ~ ($l.base(36).lc);
        $last = $i;
    }
    $enc
}
 
sub jit-decode (Str $str) {
    $str ~~ m:g/((.*?) (<:Ll>))/;
    my $dec = '';
    my $i = 0;
    for $/.List -> $l {
        my $o = ($l[0][1].Str.parse-base(36) - 10 // 0) +
                ($l[0][0].Str.parse-base(36) // 0);
        $i += $o;
        $dec ~= @stream[$i];
    }
    $dec
}
 
my $enc = jit-encode('In my opinion, this task is pretty silly.');
 
say "Encoded\n$enc\n\nDecoded\n", jit-decode($enc);
```

#### Output:
```
Encoded
26j52dhl2Wp1Gw2WceQj1Go3MyQc1Gd52a1Gb1Ga2Wcq26o4Cwf3MiQsQn2Wglk3MhkQyeQjas3MkQn2Wd26aaQcQj

Decoded
In my opinion, this task is pretty silly.
```