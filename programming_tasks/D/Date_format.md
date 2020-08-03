[1]: https://rosettacode.org/wiki/Date_format

# [Date format][1]

```perl
use DateTime::Format;
 
my $dt = DateTime.now;
 
say strftime('%Y-%m-%d', $dt);
say strftime('%A, %B %d, %Y', $dt);
```


Rudimentary DateTime operations are built-in,
as the DateTime class itself is built into a Perl 6 compiler:

```perl
use DateTime::Format;
 
my $dt = DateTime.now;
 
say $dt.yyyy-mm-dd;
say strftime('%A, %B %d, %Y', $dt);
```