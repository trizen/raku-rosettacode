[1]: https://rosettacode.org/wiki/Just_in_time_processing_on_a_character_stream

# [Just in time processing on a character stream][1]





This is a something of a toy encoder / decoder and probably shouldn't be used for anything serious.



Default encode/decode key is 'Raku' Feed it a pass phrase at the command line to use that instead.



Will handle any visible character in the ASCII range as well as space and new-line.

```perl
#`[
Set srand to set the encode / decode "key".
Need to use the same "key" and same implementation
of Raku to encode / decode. Gain "security" by
exchanging "keys" by a second channel. Default
"key" is "Raku"
]

unit sub MAIN ($key = 'Raku');

srand $key.comb(/<.alnum>/).join.parse-base(36) % 2**63;

my @stream = (flat "\n", ' ' .. '~').roll(*);

sub jit-encode (Str $str) {
    my $i = 0;
    my $last = 0;
    my $enc = '';
    for $str.comb -> $c {
        my $h;
        my $l = '';
        ++$i until $i > 1 && $c eq @stream[$i];
        my $o = $i - $last;
        $l    = $o % 26;
        $h    = $o - $l if $o >= 26;
        $l   += 10;
        $enc ~= ($h ?? $h.base(36).uc !! '') ~ ($l.base(36).lc);
        $last = $i;
    }
    my $block = 60;
    $enc.comb($block).join: "\n"
}

sub jit-decode (Str $str is copy) {
    $str.=subst("\n", '', :g);
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

my $secret = q:to/END/;
In my opinion, this task is pretty silly.

'Twas brillig, and the slithy toves
    Did gyre and gimble in the wabe.

!@#$%^&*()_+}{[].,><\|/?'";:1234567890
END

say "== Secret: ==\n$secret";

say "\n== Encoded: ==";
say my $enc = jit-encode($secret);

say "\n== Decoded: ==";
say jit-decode($enc);
```

#### Output:
```
== Secret: ==
In my opinion, this task is pretty silly.

'Twas brillig, and the slithy toves
    Did gyre and gimble in the wabe.

!@#$%^&*()_+}{[].,><\|/?'";:1234567890


== Encoded: ==
Qv26e26q1Gi2Ww5SiQr26h3Mk1GbQy52e1Gg6Ib52kQfQk26n26l26cQm26q
2Wk26vwme52qy6Ia1GuQfa3MbQxtd26aa3MvQu2Wuat26p2Wbe2Wc1Ga26g2
6h26pQha26h4Cf26jrz7Yz3MaA4h2WxFWf52zyg2WrQn2Wj26pQyQy78x1Gd
dk4Cu26k26qaaap26j26xqQf7Yr8Op3Me3Me5Sv1Ge1Gt2WxlQz5Si1GeQg4
CjQc5Sb2WbQo1GycQr1Gm1Gy1GsQei3MrQsai1Gq2WnQdt2Wj1Gff1Gg26le
2Wd1Go9Ek1Gm9Eh2Wb1Gd52h2WdQae4Chu3MeQd1Gg1Gw4CqbEGh52u2Wr1G
t52xhvQmx

== Decoded: ==
In my opinion, this task is pretty silly.

'Twas brillig, and the slithy toves
    Did gyre and gimble in the wabe.

!@#$%^&*()_+}{[].,><\|/?'";:1234567890
```
