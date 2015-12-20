[1]: http://rosettacode.org/wiki/Vector_products

# [Vector products][1]

```perl
sub infix:<⋅> { [+] @^a »*« @^b }
 
sub infix:<⨯>([$a1, $a2, $a3], [$b1, $b2, $b3]) {
    [ $a2*$b3 - $a3*$b2,
      $a3*$b1 - $a1*$b3,
      $a1*$b2 - $a2*$b1 ];
}
 
sub scalar-triple-product { @^a ⋅ (@^b ⨯ @^c) }
sub vector-triple-product { @^a ⨯ (@^b ⨯ @^c) }
 
my @a = <3 4 5>;
my @b = <4 3 5>;
my @c = <-5 -12 -13>;
 
say (:@a, :@b, :@c);
say "a ⋅ b = { @a ⋅ @b }";
say "a ⨯ b = <{ @a ⨯ @b }>";
say "a ⋅ (b ⨯ c) = { scalar-triple-product(@a, @b, @c) }";
say "a ⨯ (b ⨯ c) = <{ vector-triple-product(@a, @b, @c) }>";
```

#### Output:
```
("a" => ["3", "4", "5"], "b" => ["4", "3", "5"], "c" => ["-5", "-12", "-13"])
a ⋅ b = 49
a ⨯ b = <5 5 -7>
a ⋅ (b ⨯ c) = 6
a ⨯ (b ⨯ c) = <-267 204 -3>
```