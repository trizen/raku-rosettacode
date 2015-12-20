[1]: http://rosettacode.org/wiki/Loops/Continue

# [Loops/Continue][1]

```perl6
for 1 .. 10 {
    .print;
    if $_ %% 5 {
        print "\n";
        next;
    }
    print ', ';
}
```


or without using a loop:

```perl6
$_.join(", ").say for [1..5], [6..10];
```