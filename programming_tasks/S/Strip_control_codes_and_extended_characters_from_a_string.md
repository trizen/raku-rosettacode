[1]: http://rosettacode.org/wiki/Strip_control_codes_and_extended_characters_from_a_string

# [Strip control codes and extended characters from a string][1]

```perl6
my $str = (0..400).roll(80)».chr.join;
 
say $str;
say $str.subst(/<[ ^@..^_ ]>/, '', :g);
say $str.subst(/<-[ \ ..~ ]>/, '', :g);
```

#### Output:
```
�¶ØèúđkƌĘ�r­=êıƏÄÙÍy1SGa%TÑ�ęMRŅ�EŧİÌŬńĩµ9ŒďĔÜÉĈĬzĳdś5FúŨƏźƅíýÛÃņGÏ
                                                                      ö~ƀRÑú
¶ØèúđkƌĘr­=êıƏÄÙÍy1SGa%TÑęMRŅEŧİÌŬńĩµ9ŒďĔÜÉĈĬzĳdś5FúŨƏźƅíýÛÃņGÏö~ƀRÑú
kr=y1SGa%TMRE9zd5FG~R
```