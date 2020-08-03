[1]: https://rosettacode.org/wiki/Strip_control_codes_and_extended_characters_from_a_string

# [Strip control codes and extended characters from a string][1]

```perl
my $str = (0..400).roll(80)».chr.join;
 
say $str;
say $str.subst(/<:Cc>/,      '', :g); # unicode property: control character
say $str.subst(/<-[\ ..~]>/, '', :g);
```

#### Output:
```
kşaNĹĭŗ|Ęw��"ÄlĄWł8iCƁę��Ż�¬5ĎĶ'óü¸'ÍŸ;ŢƐ¦´ŷQċűÒŴ$ÃŅĐįð+=ĥƂ+Ōĭħ¼ŕc¤H~ìïēÕ
kşaNĹĭŗ|Ęw"ÄlĄWł8iCƁęŻ¬5ĎĶ'óü¸'ÍŸ;ŢƐ¦´ŷQċűÒŴ$ÃŅĐįð+=ĥƂ+Ōĭħ¼ŕc¤H~ìïēÕ
kaN|w"lW8iC5'';Q$+=+cH~
```