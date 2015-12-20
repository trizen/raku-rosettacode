[1]: http://rosettacode.org/wiki/Tokenize_a_string

# [Tokenize a string][1]

```perl6
'Hello,How,Are,You,Today'.split(',').join('.').say;
```


Or with function calls:

```perl6
say join '.', split ',', 'Hello,How,Are,You,Today';
```