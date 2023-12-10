[1]: https://rosettacode.org/wiki/Parameterized_SQL_statement

# [Parameterized SQL statement][1]



```perl
use DBIish;

my $db = DBIish.connect('DBI:mysql:mydatabase:host','login','password');

my $update = $db.prepare("UPDATE players SET name = ?, score = ?, active = ? WHERE jerseyNum = ?");

my $rows-affected = $update.execute("Smith, Steve",42,'true',99);
```
