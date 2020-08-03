[1]: https://rosettacode.org/wiki/Tokenize_a_string

# [Tokenize a string][1]

```raku
'Hello,How,Are,You,Today'.split(',').join('.').say;
```


Or with function calls:

```raku
say join '.', split ',', 'Hello,How,Are,You,Today';
```