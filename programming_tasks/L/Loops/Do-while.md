[1]: http://rosettacode.org/wiki/Loops/Do-while

# [Loops/Do-while][1]

```perl
my $val = 0;
repeat {
    say ++$val;
} while $val % 6;
```


`repeat ... until <i>condition</i>` is equivalent to `do ... while not <i>condition</i>`.

```perl
my $val = 0;
repeat {
    say ++$val;
} until $val %% 6;
```


(Here we've used `%%`, the "divisible-by" operator.)

```perl
my $val = 0;
repeat while $val % 6 {
    say ++$val;
}
```