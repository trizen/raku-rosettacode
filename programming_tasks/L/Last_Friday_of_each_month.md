[1]: https://rosettacode.org/wiki/Last_Friday_of_each_month

# [Last Friday of each month][1]



```perl
sub MAIN (Int $year = Date.today.year) {
    my @fri;
    for Date.new("$year-01-01") .. Date.new("$year-12-31") {
        @fri[.month] = .Str if .day-of-week == 5;
    }
    .say for @fri[1..12];
}
```


Example:


```
$ ./lastfri 2038
2038-01-29
2038-02-26
2038-03-26
2038-04-30
2038-05-28
2038-06-25
2038-07-30
2038-08-27
2038-09-24
2038-10-29
2038-11-26
2038-12-31
```


A solution without a result array to store things in:

```perl
sub MAIN (Int $year = Date.today.year) {
    say ~.value.reverse.first: *.day-of-week == 5
        for classify *.month, Date.new("$year-01-01") .. Date.new("$year-12-31");
}
```


Here, `classify` sorts the dates into one bin per month (but preserves the order in each bin). We then take the list inside each bin (`.value`) and find the last (`.reverse.first`) date which is a Friday.



Another variation where the data flow can be read left to right using feed operators:

```perl
sub MAIN (Int $year = Date.today.year) {
    .say for Date.new("$year-01-01") .. Date.new("$year-12-31") ==> classify *.month ==>
             map *.value.reverse.first: *.day-of-week == 5
}
```
