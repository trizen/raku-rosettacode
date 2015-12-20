[1]: http://rosettacode.org/wiki/Day_of_the_week

# [Day of the week][1]

As Perl 5, except `DateTime` is built-in, so you don't need to download a module of that name:

```perl6
say join ' ', grep { Date.new($_, 12, 25).day-of-week == 7 }, 2008 .. 2121;
```