[1]: http://rosettacode.org/wiki/Filter

# [Filter][1]

```perl6
my @a = 1, 2, 3, 4, 5, 6;
my @even = grep * %% 2, @a;
```


Alternatively:

```perl6
my @even = @a.grep(* %% 2);
```


Destructive:

```perl6
@a .= grep(* %% 2);
```