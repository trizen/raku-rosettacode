[1]: https://rosettacode.org/wiki/Selectively_replace_multiple_instances_of_a_character_within_a_string

# [Selectively replace multiple instances of a character within a string][1]

Set up to not particularly rely on absolute structure of the word. Demonstrate with both the original 'abracadabra' and with a random shuffled instance.

```perl
sub mangle ($str is copy) {
    $str.match(:ex, 'a')».from.map: { $str.substr-rw($_, 1) = 'ABaCD'.comb[$++] };
    $str.=subst('b', 'E');
    $str.substr-rw($_, 1) = 'F' given $str.match(:ex, 'r')».from[1];
    $str
}
 
say $_, ' -> ', .&mangle given 'abracadabra';
 
say $_, ' -> ', .&mangle given 'abracadabra'.comb.pick(*).join;
```

#### Output:
```
abracadabra -> AErBcadCbFD
caarabadrab -> cABraECdFDb
```
