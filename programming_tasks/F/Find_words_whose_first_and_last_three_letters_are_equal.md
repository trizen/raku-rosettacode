[1]: https://rosettacode.org/wiki/Find_words_whose_first_and_last_three_letters_are_equal

# [Find words whose first and last three letters are equal][1]

```perl
# 20210210 Raku programming solution

my ( \L, \N, \IN ) = 5, 3, 'unixdict.txt';

for IN.IO.lines { .say if .chars > L and .substr(0,N) eq .substr(*-N,*) }
```

#### Output:
```
antiperspirant
calendrical
einstein
hotshot
murmur
oshkosh
tartar
testes
```
