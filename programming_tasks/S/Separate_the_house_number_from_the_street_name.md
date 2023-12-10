[1]: https://rosettacode.org/wiki/Separate_the_house_number_from_the_street_name

# [Separate the house number from the street name][1]


An unquestioning translation of the Scala example's regex to show how we lay out such regexes for readability in Raku, except that we take the liberty of leaving the space out of the house number.  
(Hard constants like 1940 and 1945 are a code smell, 
and the task should probably not require such constants unless there is a standard to point to that mandates them.) 
So expect this solution to change if the task is actually defined reasonably, such as by specifying that four-digit house numbers are excluded in Europe.  
(In contrast, four- and five-digit house numbers are not uncommon 
in places such as the U.S. where each block gets a hundred house numbers 
to play with, and there are cities with hundreds of blocks along a street.)

```perl
say m[
    ( .*? )

    [
        \s+
        (
        | \d+ [ \- | \/ ] \d+
        | <!before 1940 | 1945> \d+ <[ a..z I . / \x20 ]>* \d*
        )
    ]?

    $
] for lines;
```

#### Output:
```
｢Plataanstraat 5｣
 0 => ｢Plataanstraat｣
 1 => ｢5｣

｢Straat 12｣
 0 => ｢Straat｣
 1 => ｢12｣

｢Straat 12 II｣
 0 => ｢Straat｣
 1 => ｢12 II｣

｢Dr. J. Straat   12｣
 0 => ｢Dr. J. Straat｣
 1 => ｢12｣

｢Dr. J. Straat 12 a｣
 0 => ｢Dr. J. Straat｣
 1 => ｢12 a｣

｢Dr. J. Straat 12-14｣
 0 => ｢Dr. J. Straat｣
 1 => ｢12-14｣

｢Laan 1940 – 1945 37｣
 0 => ｢Laan 1940 – 1945｣
 1 => ｢37｣

｢Plein 1940 2｣
 0 => ｢Plein 1940｣
 1 => ｢2｣

｢1213-laan 11｣
 0 => ｢1213-laan｣
 1 => ｢11｣

｢16 april 1944 Pad 1｣
 0 => ｢16 april 1944 Pad｣
 1 => ｢1｣

｢1e Kruisweg 36｣
 0 => ｢1e Kruisweg｣
 1 => ｢36｣

｢Laan 1940-’45 66｣
 0 => ｢Laan 1940-’45｣
 1 => ｢66｣

｢Laan ’40-’45｣
 0 => ｢Laan ’40-’45｣

｢Langeloërduinen 3 46｣
 0 => ｢Langeloërduinen｣
 1 => ｢3 46｣

｢Marienwaerdt 2e Dreef 2｣
 0 => ｢Marienwaerdt 2e Dreef｣
 1 => ｢2｣

｢Provincialeweg N205 1｣
 0 => ｢Provincialeweg N205｣
 1 => ｢1｣

｢Rivium 2e Straat 59.｣
 0 => ｢Rivium 2e Straat｣
 1 => ｢59.｣

｢Nieuwe gracht 20rd｣
 0 => ｢Nieuwe gracht｣
 1 => ｢20rd｣

｢Nieuwe gracht 20rd 2｣
 0 => ｢Nieuwe gracht｣
 1 => ｢20rd 2｣

｢Nieuwe gracht 20zw /2｣
 0 => ｢Nieuwe gracht｣
 1 => ｢20zw /2｣

｢Nieuwe gracht 20zw/3｣
 0 => ｢Nieuwe gracht｣
 1 => ｢20zw/3｣

｢Nieuwe gracht 20 zw/4｣
 0 => ｢Nieuwe gracht｣
 1 => ｢20 zw/4｣

｢Bahnhofstr. 4｣
 0 => ｢Bahnhofstr.｣
 1 => ｢4｣

｢Wertstr. 10｣
 0 => ｢Wertstr.｣
 1 => ｢10｣

｢Lindenhof 1｣
 0 => ｢Lindenhof｣
 1 => ｢1｣

｢Nordesch 20｣
 0 => ｢Nordesch｣
 1 => ｢20｣

｢Weilstr. 6｣
 0 => ｢Weilstr.｣
 1 => ｢6｣

｢Harthauer Weg 2｣
 0 => ｢Harthauer Weg｣
 1 => ｢2｣

｢Mainaustr. 49｣
 0 => ｢Mainaustr.｣
 1 => ｢49｣

｢August-Horch-Str. 3｣
 0 => ｢August-Horch-Str.｣
 1 => ｢3｣

｢Marktplatz 31｣
 0 => ｢Marktplatz｣
 1 => ｢31｣

｢Schmidener Weg 3｣
 0 => ｢Schmidener Weg｣
 1 => ｢3｣

｢Karl-Weysser-Str. 6｣
 0 => ｢Karl-Weysser-Str.｣
 1 => ｢6｣
```
