[1]: https://rosettacode.org/wiki/Linear_congruential_generator

# [Linear congruential generator][1]

We'll define subroutines implementing the LCG algorithm for each version. We'll make them return a lazy list.

```perl
constant modulus = 2**31;
sub bsd  {
    $^seed, ( 1103515245 * * + 12345 ) % modulus ... *
}
sub ms   {
    map * +> 16, (
	$^seed, ( 214013 * * + 2531011 ) % modulus ... *
    )
}
 
say 'BSD LCG first 10 values (first one is the seed):';
.say for bsd(0)[^10];
 
say "\nMS LCG first 10 values (first one is the seed):";
.say for ms(0)[^10];
```

#### Output:
```
BSD LCG first 10 values (first one is the seed):
0
12345
1406932606
654583775
1449466924
229283573
1109335178
1051550459
1293799192
794471793

MS LCG first 10 values (first one is the seed):
0
38
7719
21238
2437
8855
11797
8365
32285
10450
```