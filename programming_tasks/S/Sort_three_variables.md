[1]: http://rosettacode.org/wiki/Sort_three_variables

# [Sort three variables][1]

Perl 6 has a built in sort routine which uses a variation of quicksort. The built in sort routine will automatically select a numeric sort if given a list of Real numeric items and a lexical Unicode sort if given a list that contains strings. The default numeric sort won't sort complex numbers unless you give it a custom comparitor. It is trivial to modify the sort comparitor function to get whatever ordering you want though.



The list (77444, -12, 0) is a poor choice to demonstrate numeric sort since it will sort the same numerically or lexically. Instead we'll use (7.7444e4, -12, 18/2). ( A Num, an Int, and a Rat. )

```perl
# Sorting strings. Added a vertical bar between strings to make them discernable
my ($a, $b, $c) = 'lions, tigers, and', 'bears,  oh my!', '(from "The Wizard of Oz")';
say "sorting: {($a, $b, $c).join('|')}";
say ($a, $b, $c).sort.join('|'), ' - standard lexical string sort';
 
# Sorting numeric things
my ($x, $y, $z) = 7.7444e4, -12, 18/2;
say "\nsorting: $x $y $z";
say ($x, $y, $z).sort, ' - standard numeric sort, low to high';
 
# Or, with a modified comparitor:
for  -*,       ' - numeric sort high to low',
     ~*,       ' - lexical "string" sort',
     *.chars,  ' - sort by string length short to long',
     -*.chars, ' - or long to short'
  -> $comparitor, $type {
    my ($x, $y, $z) = 7.7444e4, -12, 18/2;
    say ($x, $y, $z).sort( &$comparitor ), $type;
}
say '';
 
# sort ALL THE THINGS
# sorts by lexical order with numeric values by magnitude.
.say for ($a, $b, $c, $x, $y, $z).sort;
```

#### Output:
```
sorting: lions, tigers, and|bears,  oh my!|(from "The Wizard of Oz")
(from "The Wizard of Oz")|bears,  oh my!|lions, tigers, and - standard lexical string sort

sorting: 77444 -12 9
(-12 9 77444) - standard numeric sort, low to high
(77444 9 -12) - numeric sort high to low
(-12 77444 9) - lexical "string" sort
(9 -12 77444) - sort by string length short to long
(77444 -12 9) - or long to short

(from "The Wizard of Oz")
-12
9
77444
bears,  oh my!
lions, tigers, and
```