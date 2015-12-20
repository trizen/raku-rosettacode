[1]: http://rosettacode.org/wiki/Ordered_words

# [Ordered words][1]

Here we assume the dictionary is provided on standard input.

```perl
say .value given max :by(*.key), classify *.chars, grep { [le] .comb }, lines;
```


Output:


#### Output:
```
abbott accent accept access accost almost bellow billow biopsy chilly choosy choppy effort floppy glossy knotty
```