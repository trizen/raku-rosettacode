[1]: http://rosettacode.org/wiki/The_Twelve_Days_of_Christmas

# [The Twelve Days of Christmas][1]

```perl6
my @days = <first second third fourth fifth sixth seventh eighth ninth tenth eleventh twelfth>;
 
my @gifts = lines q:to/END/;
  And a partridge in a pear tree.
  Two turtle doves,
  Three french hens,
  Four calling birds,
  Five golden rings,
  Six geese a-laying,
  Seven swans a-swimming,
  Eight maids a-milking,
  Nine ladies dancing,
  Ten lords a-leaping,
  Eleven pipers piping,
  Twelve drummers drumming,
END
 
sub nth($n) { say "On the @days[$n] day of Christmas, my true love gave to me:" }
 
nth(0);
say @gifts[0].subst('And a','A');
 
for 1 ... 11 -> $d {
    say '';
    nth($d);
    say @gifts[$_] for $d ... 0;
}
```

#### Output:
```
On the first day of Christmas, my true love gave to me:
  A partridge in a pear tree.

On the second day of Christmas, my true love gave to me:
  Two turtle doves,
  And a partridge in a pear tree.

On the third day of Christmas, my true love gave to me:
  Three french hens,
  Two turtle doves,
  And a partridge in a pear tree.

On the fourth day of Christmas, my true love gave to me:
  Four calling birds,
  Three french hens,
  Two turtle doves,
  And a partridge in a pear tree.
.
.
.
On the twelfth day of Christmas, my true love gave to me:
  Twelve drummers drumming,
  Eleven pipers piping,
  Ten lords a-leaping,
  Nine ladies dancing,
  Eight maids a-milking,
  Seven swans a-swimming,
  Six geese a-laying,
  Five golden rings,
  Four calling birds,
  Three french hens,
  Two turtle doves,
  And a partridge in a pear tree.
```