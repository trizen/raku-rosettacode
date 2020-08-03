[1]: https://rosettacode.org/wiki/Find_the_last_Sunday_of_each_month

# [Find the last Sunday of each month][1]

```perl
sub MAIN ($year = Date.today.year) {
    for 1..12 -> $month {
        my $month-end = Date.new($year, $month, Date.new($year,$month,1).days-in-month);
        say $month-end - $month-end.day-of-week % 7;
    }
}
```

#### Output:
```
2018-01-28
2018-02-25
2018-03-25
2018-04-29
2018-05-27
2018-06-24
2018-07-29
2018-08-26
2018-09-30
2018-10-28
2018-11-25
2018-12-30
```