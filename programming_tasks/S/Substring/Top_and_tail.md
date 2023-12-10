[1]: https://rosettacode.org/wiki/Substring/Top_and_tail

# [Substring/Top and tail][1]





Raku provides both functional and method forms of substr. Note that, unlike in Perl 5, offsets from the end do not use negative numbers, but instead require a function expressing the negative offset relative to the length parameter, which is supplied by the operator.  The form `*-1` is just a simple way to write such a function.



We use musical sharps and flats to illustrate that Raku is comfortable with characters from any Unicode plane.

```perl
my $s = '𝄪♯♮♭𝄫';

print qq:to/END/;
    Original:
    $s

    Remove first character:
    { substr($s, 1) }
    { $s.substr(1) }

    Remove last character:
    { substr($s, 0, *-1) }
    { $s.substr( 0, *-1) }
    { $s.chop }

    Remove first and last characters:
    { substr($s, 1, *-1) }
    { $s.substr(1, *-1) }
    END
```

#### Output:
```
Original:
𝄪♯♮♭𝄫

Remove first character:
♯♮♭𝄫
♯♮♭𝄫
 
Remove last character:
𝄪♯♮♭
𝄪♯♮♭
𝄪♯♮♭
 
Remove first and last characters:
♯♮♭
♯♮♭
```
