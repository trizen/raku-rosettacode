[1]: https://rosettacode.org/wiki/Create_a_two-dimensional_array_at_runtime

# [Create a two-dimensional array at runtime][1]

Line 1: The input parse doesn't care how you separate the dimensions as long as there are two distinct numbers.



Line 2: The list replication operator `xx` will automatically thunkify its left side so this produces new subarrays for each replication.



Line 3: Subscripting with a closure automatically passes the size of the dimension to the closure, so we pick an appropriate random index on each level.



Line 4: Print each line of the array.

```perl
my ($major,$minor) = prompt("Dimensions? ").comb(/\d+/);
my @array = [ '@' xx $minor ] xx $major;
@array[ *.rand ][ *.rand ] = ' ';
.say for @array;
```


Typical run:

```text
Dimensions? 5x35
[@ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @]
[@ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @]
[@ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @]
[@ @ @ @ @ @ @ @ @ @   @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @]
[@ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @ @]
 
```


The most recent versions of Rakudo have preliminary support for 'shaped arrays'. Natively shaped arrays are a flexible feature for declaring typed, potentially multi-dimensional arrays, potentially with pre-defined
dimensions. They will make memory-efficient matrix storage and matrix operations possible.

```perl
my ($major,$minor) = +«prompt("Dimensions? ").comb(/\d+/);
my Int @array[$major;$minor] = (7 xx $minor ) xx $major;
@array[$major div 2;$minor div 2] = 42;
say @array;
```


Typical run:

```text
Dimensions? 3 x 10
[[7 7 7 7 7 7 7 7 7 7] [7 7 7 7 7 42 7 7 7 7] [7 7 7 7 7 7 7 7 7 7]]
```