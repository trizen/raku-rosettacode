[1]: https://rosettacode.org/wiki/Password_generator

# [Password generator][1]



```perl
my @chars =
  set('a' .. 'z'),
  set('A' .. 'Z'),
  set('0' .. '9'),
  set(<!"#$%&'()*+,-./:;<=>?@[]^_{|}~>.comb);

# bleh. unconfuse syntax highlighter. '"

sub MAIN ( Int :$l = 8, Int :$c = 1, Str :$x = '' ) {
    note 'Password length must be >= 4' and exit if $l < 4;
    note 'Can not generate fewer than 0 passwords' and exit if $c < 0;
    my $chars = [∪] @chars».=&filter;
    note 'Can not exclude an entire required character group' and exit
      if any(@chars».elems) == 0;
    for ^$c {
        my @pswd;
        @pswd.push( @chars[$_].roll ) for ^4;
        @pswd.push( $chars    .roll ) for 4 ..^ $l;
        say [~] @pswd.pick(*);
    }

    sub filter (Set $set) { $set ∖ set($x.comb) }
}

sub USAGE() {
    say qq:to/END/;
    Specify a length:              --l=8 (default 8),
    Specify a count:               --c=1 (default 1),
    Specify characters to exclude: --x=
    (must escape characters significant to the shell)
    E.G.
    {$*PROGRAM-NAME} --l=14 --c=5 --x=0O\\\"\\\'1l\\\|I
    END
}
```


**Sample output:**
Using defaults:


```
c?6!xU+u
```


With passed parameters: --l=14 --c=5 --x=0O\'\"1l\|I


```
6H~jC+5(+&H44x
+Rr}2>htHMa.Y9
t~#&N]sp_zGK2#
TcP73CJ@euFMjj
9%-tYX]z?8-xA5
```


### functional

```perl
my @char-groups =
    ['a' .. 'z'],
    ['A' .. 'Z'],
    ['0' .. '9'],
    < $ % & \ ` ~ ! * + , - . / :  ;  = ? @ ^ _  ~ [ ] ( ) { | } # ' " \< \> >.Array;

subset MinimumPasswordLength of  Int where * >= 4;
subset NumberOfPasswords     of UInt where * != 0;

sub MAIN( NumberOfPasswords :c(:$count) = 1, MinimumPasswordLength :l(:$length) = 8, Str :x(:$exclude) = '' ) {
    &USAGE() if 1 == (.comb ∖ $exclude.comb).elems for @char-groups; 
    .say for password-characters($length, $exclude )
        .map( *.split(' ') )
        .map( *.pick: Inf ) # shuffle, so we don't get a predictable pattern
        .map( *.join )
        .head( $count );
}

sub password-characters( $len, $exclude ) {
    ( (( char-groups($exclude)       xx Inf ).map: *.pick).batch(     4)
     Z~
      (( char-groups($exclude, $len) xx Inf ).map: *.pick).batch($len-4) )
}

multi char-groups( $exclude )              { | @char-groups.map( * (-) $exclude.comb ) }
multi char-groups( $exclude, $max-weight ) { flat (char-groups($exclude)>>.keys.map: {$_ xx ^$max-weight .roll}) }

sub USAGE() {
    say qq:to/END/;
    Specify a length:              -l=10    (minimum 4)
    Specify a count:               -c=5     (minimum 1)
    Specify characters to exclude: -x=xkcd  (optional)
    Password must have at least one of each: lowercase letter, uppercase letter, digit, punctuation.
   END
}
```


**Sample output:**



Without parameters:


```
d[G2r4;i
```


With passed parameters: -c=5 -l=12 -x=aeiou


```
x7)YbEZQ2xp2
CEpZ>#4'rO7d
pn(5B;wb66DM
KA;3T7=s+I5{
LL<tB~L1~Y*q
```
