[1]: https://rosettacode.org/wiki/Largest_five_adjacent_number

# [Largest five adjacent number][1]

Show minimum too because... why not?



Use some Tamil Unicode numbers for brevity, and for amusement purposes.


```
   ௰ - Tamil number ten
   ௲ - Tamil number one thousand
```


Do it 5 times for variety, it's random after all.

```perl
(^௰).roll(௲).rotor(5 => -4)».join.minmax.bounds.put xx 5
```

#### Output:
```
00371 99975
00012 99982
00008 99995
00012 99945
00127 99972
```
