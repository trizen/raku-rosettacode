[1]: https://rosettacode.org/wiki/Unicode_variable_names

# [Unicode variable names][1]

 
Raku is written in Unicode so, with narrow restrictions, nearly any Unicode letter can be used in identifiers.



See the current Raku documentation on the topic here: [https://docs.raku.org/language/syntax#Identifiers](https://docs.raku.org/language/syntax#Identifiers)

```perl
my $Δ = 1;
$Δ++;
say $Δ;
```


Function and subroutine names can also use Unicode characters: (as can methods, classes, packages, whatever...)

```perl
my @ᐁ = (0, 45, 60, 90);

sub π { pi };

sub postfix:<°>($degrees) { $degrees * π / 180 };

for @ᐁ -> $ಠ_ಠ { say sin $ಠ_ಠ° };

sub c͓͈̃͂̋̈̆̽h̥̪͕ͣ͛̊aͨͣ̍͞ơ̱͔̖͖̑̽ș̻̥ͬ̃̈ͩ { 'HE COMES' }
```







**See Also:**

[Egyptian division](https://rosettacode.org/wiki/Egyptian_division#More_.22Egyptian.22_version)
