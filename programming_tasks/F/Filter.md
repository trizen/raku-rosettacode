[1]: https://rosettacode.org/wiki/Filter

# [Filter][1]

```perl
my @a = 1..6;
my @even = grep * %% 2, @a;
```


Alternatively:

```perl
my @a = 1..6;
my @even = @a.grep(* %% 2);
```


Destructive:

```perl
my @a = 1..6;
@a .= grep(* %% 2);
```