[1]: https://rosettacode.org/wiki/Bifid_cipher

# [Bifid cipher][1]

Technically incorrect as the third part doesn't "Convert ... to upper case and ignore spaces".

```perl
sub polybius ($text) {
    my $n = $text.chars.sqrt.narrow;
    $text.comb.kv.map: { $^v => ($^k % $n, $k div $n).join: ' ' }
}

sub encrypt ($message, %poly) {
    %poly.invert.hash{(flat reverse [Z] %poly{$message.comb}».words).batch(2)».reverse».join: ' '}.join
}

sub decrypt ($message, %poly) {
   %poly.invert.hash{reverse [Z] (reverse flat %poly{$message.comb}».words».reverse).batch($message.chars)}.join
}


for 'ABCDEFGHIKLMNOPQRSTUVWXYZ', 'ATTACKATDAWN',
    'BGWKZQPNDSIOAXEFCLUMTHYVR', 'FLEEATONCE',
    (flat '_', '.', 'A'..'Z', 'a'..'z', 0..9).pick(*).join, 'The invasion will start on the first of January 2023.'.subst(/' '/, '_', :g)
 -> $polybius, $message {
    my %polybius = polybius $polybius;
    say "\nUsing polybius:\n\t" ~ $polybius.comb.batch($polybius.chars.sqrt.narrow).join: "\n\t";
    say "\n  Message : $message";
    say "Encrypted : " ~ my $encrypted = encrypt $message, %polybius;
    say "Decrypted : " ~ decrypt $encrypted, %polybius;
}
```

#### Output:
```
Using polybius:
        A B C D E
        F G H I K
        L M N O P
        Q R S T U
        V W X Y Z

  Message : ATTACKATDAWN
Encrypted : DQBDAXDQPDQH
Decrypted : ATTACKATDAWN

Using polybius:
        B G W K Z
        Q P N D S
        I O A X E
        F C L U M
        T H Y V R

  Message : FLEEATONCE
Encrypted : UAEOLWRINS
Decrypted : FLEEATONCE

Using polybius:
        H c F T N 5 f i
        _ U k R B Z V W
        3 G e v s w j x
        q S 2 8 y Q . O
        m 0 E d h D r u
        M p 7 Y 4 A 9 a
        t X l I 6 g b z
        J P n 1 K L C o

  Message : The_invasion_will_start_on_the_first_of_January_2023.
Encrypted : NGiw3okfXj4XoVE_NjWcLK4Sy28EivKo3aeNiti3N3z6HCHno6Fkf
Decrypted : The_invasion_will_start_on_the_first_of_January_2023.
```
