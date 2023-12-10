[1]: https://rosettacode.org/wiki/Date_format

# [Date format][1]



```perl
use DateTime::Format;

my $dt = DateTime.now;

say strftime('%Y-%m-%d', $dt);
say strftime('%A, %B %d, %Y', $dt);
```


The built-in Date and DateTime classes both offer support for the ISO format:

```perl
my $d = Date.today;

say $d.yyyy-mm-dd;
```


They don't include the longer format specified in the task, but you can always roll your own formatter instead of importing the library:

```perl
my @months = <January February March April May June July
              August September October November December>;
my @days = <Monday Tuesday Wednesday Thursday Friday Saturday Sunday>;
my $us-long = sub ($self) { "{@days[$self.day-of-week - 1]}, {@months[$self.month - 1]} {$self.day}, {$self.year}" };
my $d = Date.today :formatter($us-long);

say $d.yyyy-mm-dd; # still works
say $d; # uses our formatter sub
```
