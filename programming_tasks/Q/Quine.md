[1]: https://rosettacode.org/wiki/Quine

# [Quine][1]

```raku
my &f = {say $^s, $^s.perl;}; f "my \&f = \{say \$^s, \$^s.perl;}; f "
 
```


Note the terminating newline.








A more compact, but still purely functional, approach:

```raku
{.fmt($_).say}(<{.fmt($_).say}(<%s>)>)
 
```


Note again the terminating newline.