[1]: https://rosettacode.org/wiki/Loops/Continue

# [Loops/Continue][1]

```perl
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

```perl
$_.join(", ").say for [1..5], [6..10];
```