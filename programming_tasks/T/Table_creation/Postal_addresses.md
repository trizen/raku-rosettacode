[1]: https://rosettacode.org/wiki/Table_creation/Postal_addresses

# [Table creation/Postal addresses][1]

Like Perl DBI, Perl 6 DBIish supports many different databases. An example using SQLite is shown here.

```perl
use DBIish;
 
my $dbh = DBIish.connect('SQLite', :database<addresses.sqlite3>);
 
my $sth = $dbh.do(q:to/STATEMENT/);
    DROP TABLE IF EXISTS Address;
    CREATE TABLE Address (
        addrID      INTEGER PRIMARY KEY AUTOINCREMENT,
        addrStreet  TEXT NOT NULL,
        addrCity    TEXT NOT NULL,
        addrState   TEXT NOT NULL,
        addrZIP     TEXT NOT NULL
    )
    STATEMENT
```