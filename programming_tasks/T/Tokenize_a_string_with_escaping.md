[1]: https://rosettacode.org/wiki/Tokenize_a_string_with_escaping

# [Tokenize a string with escaping][1]



```perl
sub tokenize ($string, :$sep!, :$esc!) {
    return $string.match(/([ <!before $sep | $esc> . | $esc . ]*)+ % $sep/)\
                  .[0].map(*.subst: /$esc )> ./, '', :g);
}

say "'$_'" for tokenize 'one^|uno||three^^^^|four^^^|^cuatro|', sep => '|', esc => '^';
```

#### Output:
```
'one|uno'
''
'three^^'
'four^|cuatro'
''
```


Notable Raku innovations that make this different from the equivalent [#Perl](#Perl) solution:
