[1]: http://rosettacode.org/wiki/Unicode_variable_names

# [Unicode variable names][1]

Perl 6 is written in Unicode so, with narrow restrictions, nearly any Unicode letter can be used in identifiers.



See Perl 6 Synopsis 02. - [http://perlcabal.org/syn/S02.html#Names](http://perlcabal.org/syn/S02.html#Names)

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
```