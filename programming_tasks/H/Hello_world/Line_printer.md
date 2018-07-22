[1]: https://rosettacode.org/wiki/Hello_world/Line_printer

# [Hello world/Line printer][1]

```perl
my $lp = open '/dev/lp0', :w;
$lp.say: 'Hello World!';
$lp.close;
```


Or using `given` to avoid having to write the variable name repeatedly:

```perl
given open '/dev/lp0', :w {
    .say: 'Hello World!';
    .close;
}
```