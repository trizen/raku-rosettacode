[1]: https://rosettacode.org/wiki/Common_sorted_list

# [Common sorted list][1]

```perl
put sort keys [∪] [5,1,3,8,9,4,8,7], [3,5,9,8,4], [1,3,7,9];
put sort keys [∪] [5,1,3,8,9,4,8,7], [3,5,9,8,4], [1,3,7,9], [<not everything is an integer so why not avoid special cases # ~ 23 4.2>];
```

#### Output:
```
1 3 4 5 7 8 9
# 1 3 4 4.2 5 7 8 9 23 an avoid cases everything integer is not so special why ~
```
