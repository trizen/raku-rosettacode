[1]: http://rosettacode.org/wiki/Split_a_character_string_based_on_change_of_character

# [Split a character string based on change of character][1]

Accept a string at the command line to split; if none provided, use default.

```perl
my $string = @*ARGS[0] // < gHHH5YY++///\ >;
put 'Original: ', $string;
put '   Split: ', join ', ', $string ~~ m:g/(.)$0*/;
```

#### Output:
```
Original: gHHH5YY++///\
   Split: g, HHH, 5, YY, ++, ///, \
```


Note that Perl 6 works with Unicode natively and handles combiners and zero width characters correctly.
