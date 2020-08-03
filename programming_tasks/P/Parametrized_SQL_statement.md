[1]: https://rosettacode.org/wiki/Parametrized_SQL_statement

# [Parametrized SQL statement][1]

```raku
use DBIish;
 
my $db = DBIish.connect('DBI:mysql:mydatabase:host','login','password');
 
my $update = $db.prepare("UPDATE players SET name = ?, score = ?, active = ? WHERE jerseyNum = ?");
 
my $rows-affected = $update.execute("Smith, Steve",42,'true',99);
```