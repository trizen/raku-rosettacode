[1]: http://rosettacode.org/wiki/Department_Numbers

# [Department Numbers][1]

```perl
for (1..7).combinations(3).grep(*.sum == 12) {
    for   .permutations\  .grep(*.[0] %%  2) {
        say <police fire sanitation> Z=> .list;
    }
}
 
```

#### Output:
```
(police => 4 fire => 1 sanitation => 7)
(police => 4 fire => 7 sanitation => 1)
(police => 6 fire => 1 sanitation => 5)
(police => 6 fire => 5 sanitation => 1)
(police => 2 fire => 3 sanitation => 7)
(police => 2 fire => 7 sanitation => 3)
(police => 2 fire => 4 sanitation => 6)
(police => 2 fire => 6 sanitation => 4)
(police => 4 fire => 2 sanitation => 6)
(police => 4 fire => 6 sanitation => 2)
(police => 6 fire => 2 sanitation => 4)
(police => 6 fire => 4 sanitation => 2)
(police => 4 fire => 3 sanitation => 5)
(police => 4 fire => 5 sanitation => 3)
```