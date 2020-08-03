[1]: https://rosettacode.org/wiki/Filter

# [Filter][1]

```raku
my @a = 1..6;
my @even = grep * %% 2, @a;
```


Alternatively:

```raku
my @a = 1..6;
my @even = @a.grep(* %% 2);
```


Destructive:

```raku
my @a = 1..6;
@a .= grep(* %% 2);
```