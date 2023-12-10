[1]: https://rosettacode.org/wiki/Days_between_dates

# [Days between dates][1]


Dates are first class objects in Raku and may have arithmetic in days done directly on them.

```perl
say Date.new('2019-09-30') - Date.new('2019-01-01');

say Date.new('2019-03-01') - Date.new('2019-02-01');

say Date.new('2020-03-01') - Date.new('2020-02-01');

say Date.new('2029-03-29') - Date.new('2019-03-29');

say Date.new('2019-01-01') + 90;

say Date.new('2020-01-01') + 90;

say Date.new('2019-02-29') + 30;

CATCH { default { .message.say; exit; } };
```

#### Output:
```
272
28
29
3653
2019-04-01
2020-03-31
Day out of range. Is: 29, should be in 1..28
```
