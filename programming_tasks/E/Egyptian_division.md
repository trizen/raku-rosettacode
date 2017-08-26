[1]: http://rosettacode.org/wiki/Egyptian_division

# [Egyptian division][1]

Only works with positive real numbers, not negative or complex.

```perl
sub egyptian-divmod (Real $dividend is copy where * >= 0, Real $divisor where * > 0) {
    my $accumulator = 0;
    ([1, $divisor], { [.[0] + .[0], .[1] + .[1]] } … ^ *.[1] > $dividend)
      .reverse.map: { $dividend -= .[1], $accumulator += .[0] if $dividend >= .[1] }
    $accumulator, $dividend;
}
 
#TESTING
for 580,34, 578,34, 7532795332300578,235117 -> $n, $d {
    printf "%s divmod %s = %s remainder %s\n",
        $n, $d, |egyptian-divmod( $n, $d )
}
```

#### Output:
```
580 divmod 34 = 17 remainder 2
578 divmod 34 = 17 remainder 0
7532795332300578 divmod 235117 = 32038497141 remainder 81
```


As a preceding version was determined to be "let's just say ... not Egyptian" we submit an alternate which is hopefully more "Egyptian". Now only handles positive Integers up to 10 million, mostly due to limitations on Egyptian notation for numbers.



Note: if the below is just a mass of "unknown glyph" boxes, try [installing](https://www.google.com/get/noto/help/install/) Googles free [Noto Sans Egyptian Hieroglyphs font](https://www.google.com/get/noto/#sans-egyp).



This is intended to be humorous and should not be regarded as good (or even sane) programming practice. That being said, 𓂽 &amp; 𓂻 really are the ancient Egyptian symbols for addition and subtraction, and the Egyptian number notation is as accurate as possible. Everything else owes more to whimsy than rigor.

```perl
my (\𓄤, \𓄊, \𓎆, \𓄰) = (0, 1, 10, 10e7);
sub infix:<𓂽> { $^𓃠 + $^𓃟 }
sub infix:<𓂻> { $^𓃲 - $^𓆊 }
sub infix:<𓈝> { $^𓃕 < $^𓃢 }
sub 𓁶 (Int \𓆉) {
    my \𓁢 = [«'' 𓏺 𓏻 𓏼 𓏽 𓏾 𓏿 𓐀 𓐁 𓐂»], [«'' 𓎆 𓎏 𓎐 𓎑 𓎊 𓎋 𓎌 𓎍 𓎎»],
      [«'' 𓍢 𓍣 𓍤 𓍥 𓍦 𓍧 𓍨 𓍩 𓍪»], [«'' 𓆼 𓆽 𓆾 𓆿 𓇀 𓇁 𓇂 𓇃 𓇄»],
      [«'' 𓂭 𓂮 𓂯 𓂰 𓂱 𓂲 𓂳 𓂴 𓂵»], ['𓆐' Xx ^𓎆], ['𓁨' Xx ^𓎆];
    ([~] 𓆉.polymod( 𓎆 xx * ).map( { 𓁢[$++;$_] } ).reverse) || '𓄤'
}
 
sub infix:<𓅓> (Int $𓂀 is copy where 𓄤 𓂻 𓄊 𓈝 * 𓈝 𓄰, Int \𓌳 where 𓄤 𓈝 * 𓈝 𓄰) {
    my $𓎦 = 𓄤;
    ([𓄊,𓌳], { [.[𓄤] 𓂽 .[𓄤], .[𓄊] 𓂽 .[𓄊]] } … ^$𓂀 𓈝 *.[𓄊])
      .reverse.map: { $𓂀 𓂻= .[𓄊], $𓎦 𓂽= .[𓄤] if .[𓄊] 𓈝 ($𓂀 𓂽 𓄊) }
    $𓎦, $𓂀;
}
 
#TESTING
for 580,34, 578,34, 2300578,23517 -> \𓃾, \𓆙 {
    printf "%s divmod %s = %s remainder %s =OR= %s 𓅓 %s = %s remainder %s\n",
        𓃾, 𓆙, |(𓃾 𓅓 𓆙), (𓃾, 𓆙, |(𓃾 𓅓 𓆙))».&𓁶;
}
```

#### Output:
```
580 divmod 34 = 17 remainder 2 =OR= 𓍦𓎍 𓅓 𓎐𓏽 = 𓎆𓐀 remainder 𓏻
578 divmod 34 = 17 remainder 0 =OR= 𓍦𓎌𓐁 𓅓 𓎐𓏽 = 𓎆𓐀 remainder 𓄤
2300578 divmod 23517 = 97 remainder 19429 =OR= 𓁨𓁨𓆐𓆐𓆐𓍦𓎌𓐁 𓅓 𓂮𓆾𓍦𓎆𓐀 = 𓎎𓐀 remainder 𓂭𓇄𓍥𓎏𓐂
```