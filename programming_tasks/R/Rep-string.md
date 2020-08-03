[1]: https://rosettacode.org/wiki/Rep-string

# [Rep-string][1]

```raku
for <1001110011 1110111011 0010010010 1010101010 1111111111 0100101101 0100100 101 11 00 1> {
    if /^ (.+) $0+: (.*$) <?{ $0.substr(0,$1.chars) eq $1 }> / {
	my $rep = $0.chars;
	say .substr(0,$rep), .substr($rep,$rep).trans('01' => '𝟘𝟙'), .substr($rep*2);
    }
    else {
	say "$_ (no repeat)";
    }
}
```

#### Output:
```
10011𝟙𝟘𝟘𝟙𝟙
1110𝟙𝟙𝟙𝟘11
001𝟘𝟘𝟙0010
1010𝟙𝟘𝟙𝟘10
11111𝟙𝟙𝟙𝟙𝟙
0100101101 (no repeat)
010𝟘𝟙𝟘0
101 (no repeat)
1𝟙
0𝟘
1 (no repeat)
```


Here's a technique that relies on the fact that XORing the shifted binary number
should set all the lower bits to 0 if there are repeats.
(The cool thing is that shift will automatically
throw away the bits on the right that you want thrown away.)
This produces the same output as above.

```raku
sub repstr(Str $s) {
    my $bits = :2($s);
    for reverse 1 .. $s.chars div 2 -> $left {
	my $right = $s.chars - $left;
	return $left if $bits +^ ($bits +> $left) == $bits +> $right +< $right;
    }
}
 
 
for '1001110011 1110111011 0010010010 1010101010 1111111111 0100101101 0100100 101 11 00 1'.words {
    if repstr $_ -> $rep {
	say .substr(0,$rep), .substr($rep,$rep).trans('01' => '𝟘𝟙'), .substr($rep*2);
    }
    else {
	say "$_ (no repeat)";
    }
}
```