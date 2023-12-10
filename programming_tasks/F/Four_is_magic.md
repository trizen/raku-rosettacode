[1]: https://rosettacode.org/wiki/Four_is_magic

# [Four is magic][1]





Lingua::EN::Numbers module available from the [Raku ecosystem](https://modules.raku.org/search/?q=Lingua%3A%3AEN%3A%3ANumbers).

```perl
use Lingua::EN::Numbers; # Version 2.4.0 or higher

sub card ($n) { cardinal($n).subst(/','/, '', :g) }

sub magic (Int $int is copy) {
    my $string;
    loop {
       $string ~= "{ card($int) } is ";
       if $int = ($int == 4) ?? 0 !! card($int).chars {
           $string ~= "{ card($int) }, "
       } else {
           $string ~= "magic.\n";
           last
       }
   }
   $string.tc
}

.&magic.say for 0, 4, 6, 11, 13, 75, 337, -164, 9876543209, 2**256;
```

#### Output:
```
Zero is four, four is magic.

Four is magic.

Six is three, three is five, five is four, four is magic.

Eleven is six, six is three, three is five, five is four, four is magic.

Thirteen is eight, eight is five, five is four, four is magic.

Seventy-five is twelve, twelve is six, six is three, three is five, five is four, four is magic.

Three hundred thirty-seven is twenty-six, twenty-six is ten, ten is three, three is five, five is four, four is magic.

Negative one hundred sixty-four is thirty-one, thirty-one is ten, ten is three, three is five, five is four, four is magic.

Nine billion eight hundred seventy-six million five hundred forty-three thousand two hundred nine is ninety-seven, ninety-seven is twelve, twelve is six, six is three, three is five, five is four, four is magic.

One hundred fifteen quattuorvigintillion seven hundred ninety-two trevigintillion eighty-nine duovigintillion two hundred thirty-seven unvigintillion three hundred sixteen vigintillion one hundred ninety-five novemdecillion four hundred twenty-three octodecillion five hundred seventy septendecillion nine hundred eighty-five sexdecillion eight quindecillion six hundred eighty-seven quattuordecillion nine hundred seven tredecillion eight hundred fifty-three duodecillion two hundred sixty-nine undecillion nine hundred eighty-four decillion six hundred sixty-five nonillion six hundred forty octillion five hundred sixty-four septillion thirty-nine sextillion four hundred fifty-seven quintillion five hundred eighty-four quadrillion seven trillion nine hundred thirteen billion one hundred twenty-nine million six hundred thirty-nine thousand nine hundred thirty-six is eight hundred sixty-nine, eight hundred sixty-nine is twenty-four, twenty-four is eleven, eleven is six, six is three, three is five, five is four, four is magic.
```
