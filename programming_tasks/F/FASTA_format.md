[1]: https://rosettacode.org/wiki/FASTA_format

# [FASTA format][1]

```perl
grammar FASTA {
 
    rule TOP    { <entry>+ }
    rule entry  { \> <title> <sequence> }
    token title { <.alnum>+ }
    token sequence { ( <.alnum>+ )+ % \n { make $0.join } }
 
}
 
FASTA.parse: q:to /§/;
>Rosetta_Example_1
THERECANBENOSPACE
>Rosetta_Example_2
THERECANBESEVERAL
LINESBUTTHEYALLMUST
BECONCATENATED
§
 
for $/<entry>[] {
    say ~.<title>, " : ", .<sequence>.made;
}
```

#### Output:
```
Rosetta_Example_1 : THERECANBENOSPACE
Rosetta_Example_2 : THERECANBESEVERALLINESBUTTHEYALLMUSTBECONCATENATED
```