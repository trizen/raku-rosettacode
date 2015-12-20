[1]: http://rosettacode.org/wiki/Zeckendorf_arithmetic

# [Zeckendorf arithmetic][1]

This is a somewhat limited implementation of Zeckendorf arithmetic operators. They only handle positive integer values. There are no actual calculations, everything is done with string manipulations, so it doesn't matter what glyphs you use for 1 and 0.



Implemented arithmetic operators:


#### Output:
```
 addition: <b>+z</b>
 subtraction: <b>-z</b>
 multiplication: <b>*z</b>
 division: <b>/z</b> (more of a divmod really)
 post increment: <b>++z</b>
 post decrement: <b>--z</b>
```


Comparison operators:


#### Output:
```
 equal <b>eqz</b>
 not equal <b>nez</b>
 greater than <b>gtz</b>
 less than <b>ltz</b>
```
```perl
 
my $z1 = '1'; # glyph to use for a '1'
my $z0 = '0'; # glyph to use for a '0'
 
sub zorder($a) { ($z0 lt $z1) ?? $a !! $a.trans([$z0, $z1] => [$z1, $z0]) };
 
######## Zeckendorf comparison operators #########
 
# less than
sub infix:<ltz>($a, $b) { $a.&zorder lt $b.&zorder };
 
# greater than
sub infix:<gtz>($a, $b) { $a.&zorder gt $b.&zorder };
 
# equal
sub infix:<eqz>($a, $b) { $a eq $b };
 
# not equal
sub infix:<nez>($a, $b) { $a ne $b };
 
 
######## Operators for Zeckendorf arithmetic ########
 
# post increment
sub postfix:<++z>($a is rw) {
    $a = ("$z0$z0"~$a).subst(/("$z0$z0")($z1+ %% $z0)?$/,
      -> $/ { "$z0$z1" ~ ($1 ?? $z0 x $1.chars !! '') });
    $a ~~ s/^$z0+//;
    $a
}
 
# post decrement
sub postfix:<--z>($a is rw) {
    $a.=subst(/$z1($z0*)$/,
      -> $/ {$z0 ~ "$z1$z0" x $0.chars div 2 ~ $z1 x $0.chars mod 2});
    $a ~~ s/^$z0+(.+)$/$0/;
    $a
}
 
# addition
sub infix:<+z>($a is copy, $b is copy) { $a++z while $b--z nez $z0; $a };
 
# subtraction
sub infix:<-z>($a is copy, $b is copy) { $a--z while $b--z nez $z0; $a };
 
# multiplication
sub infix:<*z>($a, $b) { 
    return $z0 if $a eqz $z0 or $b eqz $z0;
    return $a if $b eqz $z1;
    return $b if $a eqz $z1;
    my $c = $a;
    my $d = $z1;
    repeat { 
         my $e = $z0;
         repeat { $c++z; $e++z } until $e eqz $a;
         $d++z;
    } until $d eqz $b;
    $c
};
 
# division  (really more of a div mod)
sub infix:</z>($a is copy, $b is copy) {
    fail "Divide by zero" if $b eqz $z0;
    return $a if $a eqz $z0 or $b eqz $z1;
    my $c = $z0;
    repeat { 
        my $d = $b +z ($z1 ~ $z0);
        $c++z;
        $a--z while $d--z nez $z0
    } until $a ltz $b;
    $c ~= " remainder $a" if $a nez $z0;
    $c
};
 
 
###################### Testing ######################
 
# helper sub to translate constants into the particular glyphs you used
sub z($a) { $a.trans([<1 0>] => [$z1, $z0]) };
 
say "Using the glyph '$z1' for 1 and '$z0' for 0\n";
 
my $fmt = "%-22s = %15s  %s\n";
 
my $zeck = $z1;
 
printf( $fmt, "$zeck++z", $zeck++z, '# increment' ) for 1 .. 10;
 
printf $fmt, "$zeck +z {z('1010')}", $zeck +z= z('1010'), '# addition';
 
printf $fmt, "$zeck -z {z('100')}", $zeck -z= z('100'), '# subtraction';
 
printf $fmt, "$zeck *z {z('100101')}", $zeck *z= z('100101'), '# multiplication';
 
printf $fmt, "$zeck /z {z('100')}", $zeck /z= z('100'), '# division';
 
printf( $fmt, "$zeck--z", $zeck--z, '# decrement' ) for 1 .. 5;
 
printf $fmt, "$zeck *z {z('101001')}", $zeck *z= z('101001'), '# multiplication';
 
printf $fmt, "$zeck /z {z('100')}", $zeck /z= z('100'), '# division';
```


*Testing Output*


#### Output:
```
Using the glyph '1' for 1 and '0' for 0

1++z                   =              10  # increment
10++z                  =             100  # increment
100++z                 =             101  # increment
101++z                 =            1000  # increment
1000++z                =            1001  # increment
1001++z                =            1010  # increment
1010++z                =           10000  # increment
10000++z               =           10001  # increment
10001++z               =           10010  # increment
10010++z               =           10100  # increment
10100 +z 1010          =          100101  # addition
100101 -z 100          =          100010  # subtraction
100010 *z 100101       =    100001000001  # multiplication
100001000001 /z 100    =       101010001  # division
101010001--z           =       101010000  # decrement
101010000--z           =       101001010  # decrement
101001010--z           =       101001001  # decrement
101001001--z           =       101001000  # decrement
101001000--z           =       101000101  # decrement
101000101 *z 101001    = 101010000010101  # multiplication
101010000010101 /z 100 = 1001010001001 remainder 10  # division
```


Output using 'X' for 1 and 'O' for 0:


#### Output:
```
Using the glyph 'X' for 1 and 'O' for 0

X++z                   =              XO  # increment
XO++z                  =             XOO  # increment
XOO++z                 =             XOX  # increment
XOX++z                 =            XOOO  # increment
XOOO++z                =            XOOX  # increment
XOOX++z                =            XOXO  # increment
XOXO++z                =           XOOOO  # increment
XOOOO++z               =           XOOOX  # increment
XOOOX++z               =           XOOXO  # increment
XOOXO++z               =           XOXOO  # increment
XOXOO +z XOXO          =          XOOXOX  # addition
XOOXOX -z XOO          =          XOOOXO  # subtraction
XOOOXO *z XOOXOX       =    XOOOOXOOOOOX  # multiplication
XOOOOXOOOOOX /z XOO    =       XOXOXOOOX  # division
XOXOXOOOX--z           =       XOXOXOOOO  # decrement
XOXOXOOOO--z           =       XOXOOXOXO  # decrement
XOXOOXOXO--z           =       XOXOOXOOX  # decrement
XOXOOXOOX--z           =       XOXOOXOOO  # decrement
XOXOOXOOO--z           =       XOXOOOXOX  # decrement
XOXOOOXOX *z XOXOOX    = XOXOXOOOOOXOXOX  # multiplication
XOXOXOOOOOXOXOX /z XOO = XOOXOXOOOXOOX remainder XO  # division
```