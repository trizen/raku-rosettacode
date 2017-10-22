[1]: http://rosettacode.org/wiki/Four_is_magic

# [Four is magic][1]

Lingua::EN::Numbers::Cardinal module available from the [Perl 6 ecosystem](https://modules.perl6.org/search/?q=Lingua%3A%3AEN%3A%3ANumbers%3A%3ACardinal).

```perl
use Lingua::EN::Numbers::Cardinal;
 
sub card ($n) { cardinal($n).subst(/','/, '', :g) }
 
sub magic (Int $int is copy) {
    my $string;
    loop {
       $string ~= "{ card($int) } is ";
       if $int = ($int == 4) ?? 0 !! card($int).chars {
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