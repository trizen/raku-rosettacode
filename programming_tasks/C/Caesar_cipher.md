[1]: https://rosettacode.org/wiki/Caesar_cipher

# [Caesar cipher][1]



```perl
my @alpha = 'A' .. 'Z';
sub encrypt ( $key where 1..25, $plaintext ) {
    $plaintext.trans( @alpha Z=> @alpha.rotate($key) );
}
sub decrypt ( $key where 1..25, $cyphertext ) {
    $cyphertext.trans( @alpha.rotate($key) Z=> @alpha );
}

my $original = 'THE FIVE BOXING WIZARDS JUMP QUICKLY';
my $en = encrypt( 13, $original );
my $de = decrypt( 13, $en );

.say for $original, $en, $de;

say 'OK' if $original eq all( map { .&decrypt(.&encrypt($original)) }, 1..25 );
```

#### Output:
```
THE FIVE BOXING WIZARDS JUMP QUICKLY
GUR SVIR OBKVAT JVMNEQF WHZC DHVPXYL
THE FIVE BOXING WIZARDS JUMP QUICKLY
OK
```
