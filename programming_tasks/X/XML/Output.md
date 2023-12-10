[1]: https://rosettacode.org/wiki/XML/Output

# [XML/Output][1]



```perl
use XML::Writer;

my @students =
    [ Q[April],         Q[Bubbly: I'm > Tam and <= Emily] ],
    [ Q[Tam O'Shanter], Q[Burns: "When chapman billies leave the street ..."] ],
    [ Q[Emily],         Q[Short & shrift] ]
;

my @lines = map { :Character[:name(.[0]), .[1]] }, @students;

say XML::Writer.serialize( CharacterRemarks => @lines );
```
```xml
<CharacterRemarks><Character name="April">Bubbly: I'm &gt; Tam and &lt;= Emily</Character>
<Character name="Tam O'Shanter">Burns: &quot;When chapman billies leave the street ...&quot;</Character>
<Character name="Emily">Short &amp; shrift</Character></CharacterRemarks>
```
