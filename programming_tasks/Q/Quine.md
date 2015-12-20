[1]: http://rosettacode.org/wiki/Quine

# [Quine][1]

```perl
my &f = {say $^s, $^s.perl;}; f "my \&f = \{say \$^s, \$^s.perl;}; f "
 
```


Note the terminating newline.








A more compact, but still purely functional, approach:

```perl
{.fmt($_).say}(<{.fmt($_).say}(<%s>)>)
 
```


Note again the terminating newline.