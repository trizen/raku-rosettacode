[1]: https://rosettacode.org/wiki/Five_weekends

# [Five weekends][1]



```perl
# A month has 5 weekends iff it has 31 days and starts on Friday.

my @years = 1900 .. 2100;
my @has31 = 1, 3, 5, 7, 8, 10, 12;
my @happy = ($_ when *.day-of-week == 5 for (@years X @has31).map(-> ($y, $m) { Date.new: $y, $m, 1 }));

say 'Happy month count:  ', +@happy;
say 'First happy months: ' ~ @happy[^5];
say 'Last  happy months: ' ~ @happy[*-5 .. *];
say 'Dreary years count: ',  @years - @happy».year.squish;
```

#### Output:
```
Happy month count:  201
First happy months: 1901-03-01 1902-08-01 1903-05-01 1904-01-01 1904-07-01
Last  happy months: 2097-03-01 2098-08-01 2099-05-01 2100-01-01 2100-10-01
Dreary years count: 29
```
