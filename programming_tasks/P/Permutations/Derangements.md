[1]: http://rosettacode.org/wiki/Permutations/Derangements

# [Permutations/Derangements][1]

```perl6
sub derange (@result, @avail) {
    if not @avail { @result.item }
    else {
	map {
	    derange([ @result, @avail[$_] ], 
	      @avail[0 .. $_-1, $_+1 ..^ @avail ])
	}, grep { @avail[$_] != @result }, 0 .. @avail-1;
    }
}
 
constant factorial = 1, [\*] 1...*;
 
# choose k among n, i.e. n! / k! (n-k)!
sub choose ($n, $k) { factorial[$n] div factorial[$k] div factorial[$n - $k] }
 
sub sub-factorial ($n) {
    (state @)[$n] //=
	factorial[$n] - [+] gather for 1 .. $n -> $k {
	    take choose($n, $k) * sub-factorial($n - $k);
	}
}
 
say "Derangements for 4 elements:";
for derange([], 0 .. 3).kv -> $i, @d {
    say $i+1, ': ', @d;
}
 
say "\nCompare list length and calculated table";
say "$_\t{+derange([], ^$_)}\t{sub-factorial($_)}" for 0 .. 9;
 
say "\nNumber of derangements:";
say "$_:\t{sub-factorial($_)}" for 1 .. 20;
```

#### Output:
```
Derangements for 4 elements:
1: 1 0 3 2
2: 1 2 3 0
3: 1 3 0 2
4: 2 0 3 1
5: 2 3 0 1
6: 2 3 1 0
7: 3 0 1 2
8: 3 2 0 1
9: 3 2 1 0

Compare list length and calculated table
0       1       1
1       0       0
2       1       1
3       2       2
4       9       9
5       44      44
6       265     265
7       1854    1854
8       14833   14833
9       133496  133496

Number of derangements:
1:      0
2:      1
3:      2
4:      9
5:      44
6:      265
7:      1854
8:      14833
9:      133496
10:     1334961
11:     14684570
12:     176214841
13:     2290792932
14:     32071101049
15:     481066515734
16:     7697064251745
17:     130850092279664
18:     2355301661033953
19:     44750731559645106
20:     895014631192902121
```