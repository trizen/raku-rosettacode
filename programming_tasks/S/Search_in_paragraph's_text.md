[1]: https://rosettacode.org/wiki/Search_in_paragraph%27s_text

# [Search in paragraph's text][1]

Generalized. Pipe in the file from the shell, user definable search term and entry point.

```perl
unit sub MAIN ( :$for, :$at = 0 );
 
put slurp.split( /\n\n+/ ).grep( { .contains: $for } )
         .map( { .substr: .index: $at } )
         .join: "\n----------------\n"
```

#### Output:
```
   raku search.raku --for='SystemError' --at='Traceback' < traceback.txt
```


*Matches expected except for not having a extraneous trailing paragraph separator.*
