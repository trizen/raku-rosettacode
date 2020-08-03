[1]: https://rosettacode.org/wiki/Ordered_words

# [Ordered words][1]

Here we assume the dictionary is provided on standard input.

```perl
say lines.grep({ [le] .comb }).classify(*.chars).max(*.key).value
```

#### Output:
```
[abbott accent accept access accost almost bellow billow biopsy chilly choosy choppy effort floppy glossy knotty]
```