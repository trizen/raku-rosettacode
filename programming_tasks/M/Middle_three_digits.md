[1]: http://rosettacode.org/wiki/Middle_three_digits

# [Middle three digits][1]

```perl6
sub middle-three($n) {
    given $n.abs {
        when .chars < 3  { "$n is too short" }
        when .chars %% 2 { "$n has an even number of digits" }
        default          { "The three middle digits of $n are: ", .substr: (.chars - 3)/2, 3 }
    }
}
 
say middle-three($_) for <
    123 12345 1234567 987654321 10001 -10001 -123 -100 100 -12345
    1 2 -1 -10 2002 -2002 0
>;
```

#### Output:
```
The three middle digits of 123 are:  123
The three middle digits of 12345 are:  234
The three middle digits of 1234567 are:  345
The three middle digits of 987654321 are:  654
The three middle digits of 10001 are:  000
The three middle digits of -10001 are:  000
The three middle digits of -123 are:  123
The three middle digits of -100 are:  100
The three middle digits of 100 are:  100
The three middle digits of -12345 are:  234
1 is too short
2 is too short
-1 is too short
-10 is too short
2002 has an even number of digits
-2002 has an even number of digits
0 is too short
```