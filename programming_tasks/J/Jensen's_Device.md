[1]: http://rosettacode.org/wiki/Jensen's_Device

# [Jensen's Device][1]

Rather than playing tricks like Perl&#160;5 does, the declarations of the formal parameters are quite straightforward in Perl&#160;6:

```perl
sub sum($i is rw, $lo, $hi, &term) {
    my $temp = 0;
    loop ($i = $lo; $i <= $hi; $i++) {
        $temp += term;
    }
    return $temp;
}
 
my $i;
say sum $i, 1, 100, { 1 / $i };
```


Note that the C-style "for" loop is pronounced "loop" in Perl&#160;6, and is the only loop statement that actually requires parens.