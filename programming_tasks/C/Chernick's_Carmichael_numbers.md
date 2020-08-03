[1]: https://rosettacode.org/wiki/Chernick's_Carmichael_numbers

# [Chernick's Carmichael numbers][1]

Use the ntheory library from Perl 5 for primality testing since it is much, *much* faster than Perl 6s built-in .is-prime method.

```raku
use Inline::Perl5;
use ntheory:from<Perl5> <:all>;
 
sub chernick-factors ($n, $m) {
    6*$m + 1, 12*$m + 1, |((1 .. $n-2).map: { (1 +< $_) * 9*$m + 1 } )
}
 
sub chernick-carmichael-number ($n) {
 
    my $multiplier = 1 +< (($n-4) max 0);
    my $iterator   = $n < 5 ?? (1 .. *) !! (1 .. *).map: * * 5;
 
    $multiplier * $iterator.first: -> $m {
        [&&] chernick-factors($n, $m * $multiplier).map: { is_prime($_) }
    }
 
}
 
for 3 .. 9 -> $n {
    my $m = chernick-carmichael-number($n);
    my @f = chernick-factors($n, $m);
    say "U($n, $m): {[*] @f} = {@f.join(' ⨉ ')}";
}
```

#### Output:
```
U(3, 1): 1729 = 7 ⨉ 13 ⨉ 19
U(4, 1): 63973 = 7 ⨉ 13 ⨉ 19 ⨉ 37
U(5, 380): 26641259752490421121 = 2281 ⨉ 4561 ⨉ 6841 ⨉ 13681 ⨉ 27361
U(6, 380): 1457836374916028334162241 = 2281 ⨉ 4561 ⨉ 6841 ⨉ 13681 ⨉ 27361 ⨉ 54721
U(7, 780320): 24541683183872873851606952966798288052977151461406721 = 4681921 ⨉ 9363841 ⨉ 14045761 ⨉ 28091521 ⨉ 56183041 ⨉ 112366081 ⨉ 224732161
U(8, 950560): 53487697914261966820654105730041031613370337776541835775672321 = 5703361 ⨉ 11406721 ⨉ 17110081 ⨉ 34220161 ⨉ 68440321 ⨉ 136880641 ⨉ 273761281 ⨉ 547522561
U(9, 950560): 58571442634534443082821160508299574798027946748324125518533225605795841 = 5703361 ⨉ 11406721 ⨉ 17110081 ⨉ 34220161 ⨉ 68440321 ⨉ 136880641 ⨉ 273761281 ⨉ 547522561 ⨉ 1095045121
```
