[1]: http://rosettacode.org/wiki/Find_the_last_Sunday_of_each_month

# [Find the last Sunday of each month][1]

```perl
sub MAIN ($year = Date.new.year) {
    for 1..12 -> $mo {
        my $month-end = Date.new($year, $mo, Date.days-in-month($year, $mo));
        say $month-end - $month-end.day-of-week % 7;
    }
}
```

#### Output:
```
2015-01-25
2015-02-22
2015-03-29
2015-04-26
2015-05-31
2015-06-28
2015-07-26
2015-08-30
2015-09-27
2015-10-25
2015-11-29
2015-12-27
```