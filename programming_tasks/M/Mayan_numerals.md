[1]: https://rosettacode.org/wiki/Mayan_numerals

# [Mayan numerals][1]





Just a matter of converting to base twenty, then divmod 5 each digit and map to the appropriate glyph. Most of this is display code to deal with the cartouche requirement.



I actually spent a little time looking into what exactly a Mayan cartouche was supposed to look like. The classic Mayan cartouche was kind of a rounded rectangle with three little semi-circles underneath. Kind of looks like a picture frame. In general, they were only used when expressing significant "holy" dates in the Mayan calender.



These don't particularly resemble the Mayan cartouche, more like mahjong tiles actually. It would have been *so* much easier and more compact if I could have put all the styling into a CSS file instead of inlining it, but oh well, it was entertaining to do.

```perl
### Formatting ###
my $t-style = '"border-collapse: separate; text-align: center; border-spacing: 3px 0px;"';
my $c-style = '"border: solid black 2px;background-color: #fffff0;border-bottom: double 6px;'~
  'border-radius: 1em;-moz-border-radius: 1em;-webkit-border-radius: 1em;'~
  'vertical-align: bottom;width: 3.25em;"';
my $joiner = '<br>';

sub display ($num, @digits) { join "\n", "\{| style=$t-style", "|+ $num", '|-', (|@digits.map: {"| style=$c-style | $_"}), '|}' }

### Logic ###

sub mayan ($int) { $int.polymod(20 xx *).reverse.map: *.polymod(5) }

my @output = <4005 8017 326205 886205 16160025 1081439556 503491211079>.map: {
  display $_, .&mayan.map: { [flat '' xx 3, '●' x .[0], '───' xx .[1], ('Θ' if !.[0,1].sum)].tail(4).join: $joiner }
}

say @output.join: "\n$joiner\n";
```





































Or, plain old text mode. Not as pretty, but still serviceable.

```perl
### Formatting ###
use Terminal::Boxer;
my $joiner = "\n";

sub display ($num, @digits) { "$num\n" ~ dd-box( :6cw, :4ch, @digits ) }

### Logic ###

sub mayan ($int) { $int.polymod(20 xx *).reverse.map: *.polymod(5) }

my @output = <4005 8017 326205 886205 16160025 1081439556 503491211079>.map: {
  display $_, .&mayan.map: { [flat '' xx 3, '●' x .[0], '────' xx .[1], ('Θ' if !.[0,1].sum)].tail(4).join: $joiner }
}

say @output.join: $joiner;
```

#### Output:
```
4005
╔══════╦══════╦══════╗
║      ║      ║      ║
║      ║      ║      ║
║ ──── ║      ║      ║
║ ──── ║   Θ  ║ ──── ║
╚══════╩══════╩══════╝

8017
╔══════╦══════╦══════╦══════╗
║      ║      ║      ║  ●●  ║
║      ║      ║      ║ ──── ║
║      ║      ║      ║ ──── ║
║   ●  ║   Θ  ║   Θ  ║ ──── ║
╚══════╩══════╩══════╩══════╝

326205
╔══════╦══════╦══════╦══════╦══════╗
║      ║      ║      ║      ║      ║
║      ║      ║ ──── ║      ║      ║
║      ║      ║ ──── ║ ──── ║      ║
║  ●●  ║   Θ  ║ ──── ║ ──── ║ ──── ║
╚══════╩══════╩══════╩══════╩══════╝

886205
╔══════╦══════╦══════╦══════╦══════╗
║      ║      ║      ║      ║      ║
║      ║      ║ ──── ║      ║      ║
║      ║ ──── ║ ──── ║ ──── ║      ║
║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║
╚══════╩══════╩══════╩══════╩══════╝

16160025
╔══════╦══════╦══════╦══════╦══════╦══════╗
║      ║      ║      ║      ║      ║      ║
║      ║      ║      ║      ║      ║      ║
║      ║      ║      ║      ║      ║      ║
║ ──── ║   ●  ║   Θ  ║   Θ  ║   ●  ║ ──── ║
╚══════╩══════╩══════╩══════╩══════╩══════╝

1081439556
╔══════╦══════╦══════╦══════╦══════╦══════╦══════╗
║   ●  ║  ●●  ║  ●●● ║ ●●●● ║  ●●● ║  ●●  ║   ●  ║
║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║
║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║
║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║ ──── ║
╚══════╩══════╩══════╩══════╩══════╩══════╩══════╝

503491211079
╔══════╦══════╦══════╦══════╦══════╦══════╦══════╦══════╦══════╗
║ ●●●● ║      ║      ║      ║      ║      ║      ║      ║ ●●●● ║
║ ──── ║  ●●● ║      ║      ║      ║      ║      ║  ●●● ║ ──── ║
║ ──── ║ ──── ║  ●●  ║      ║      ║      ║  ●●  ║ ──── ║ ──── ║
║ ──── ║ ──── ║ ──── ║   ●  ║   Θ  ║   ●  ║ ──── ║ ──── ║ ──── ║
╚══════╩══════╩══════╩══════╩══════╩══════╩══════╩══════╩══════╝
```
