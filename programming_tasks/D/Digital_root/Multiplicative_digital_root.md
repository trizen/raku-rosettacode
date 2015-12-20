[1]: http://rosettacode.org/wiki/Digital_root/Multiplicative_digital_root

# [Digital root/Multiplicative digital root][1]

```perl6
sub multiplicative-digital-root(Int $n) {
    return .elems - 1, .[.end]
        given $n, {[*] .comb} ... *.chars == 1
}
 
for 123321, 7739, 893, 899998 {
    say "$_: ", .&multiplicative-digital-root;
}
 
for ^10 -> $d {
    say "$d : ", .[^5]
        given (1..*).grep: *.&multiplicative-digital-root[1] == $d;
}
```

#### Output:
```
123321: 3 8
7739: 3 8
893: 3 2
899998: 2 0
0 : 10 20 25 30 40
1 : 1 11 111 1111 11111
2 : 2 12 21 26 34
3 : 3 13 31 113 131
4 : 4 14 22 27 39
5 : 5 15 35 51 53
6 : 6 16 23 28 32
7 : 7 17 71 117 171
8 : 8 18 24 29 36
9 : 9 19 33 91 119
```