[1]: https://rosettacode.org/wiki/Regular_expressions

# [Regular expressions][1]



```perl
if 'a long string' ~~ /string$/ {
   say "It ends with 'string'";
}

# substitution has a few nifty features

$_ = 'The quick Brown fox';
s:g:samecase/\w+/xxx/;
.say;
# output:
# Xxx xxx Xxx xxx
```
