[1]: https://rosettacode.org/wiki/Date_format

# [Date format][1]

```raku
use DateTime::Format;
 
my $dt = DateTime.now;
 
say strftime('%Y-%m-%d', $dt);
say strftime('%A, %B %d, %Y', $dt);
```


Rudimentary DateTime operations are built-in,
as the DateTime class itself is built into a Perl 6 compiler:

```raku
use DateTime::Format;
 
my $dt = DateTime.now;
 
say $dt.yyyy-mm-dd;
say strftime('%A, %B %d, %Y', $dt);
```