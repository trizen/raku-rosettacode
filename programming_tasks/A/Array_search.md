[1]: http://rosettacode.org/wiki/Array_search

# [Array search][1]

There are several search operations that may be used. It mostly depends on whether you want to find actual values or pointers, and/or all possible values or a single value matching your criteria. The most appropriate for the given test data/operations are shown here.

```perl
use JSON::Tiny;
 
my $cities = from-json('
[{"name":"Lagos", "population":21}, {"name":"Cairo", "population":15.2}, {"name":"Kinshasa-Brazzaville", "population":11.3}, {"name":"Greater Johannesburg", "population":7.55}, {"name":"Mogadishu", "population":5.85}, {"name":"Khartoum-Omdurman", "population":4.98}, {"name":"Dar Es Salaam", "population":4.7}, {"name":"Alexandria", "population":4.58}, {"name":"Abidjan", "population":4.4}, {"name":"Casablanca", "population":3.98}]
');
 
# Find the indicies of the cities named 'Dar Es Salaam'.
say grep { $_<name> eq 'Dar Es Salaam'}, :k, @$cities; # (6)
 
# Find the name of the first city with a population less
# than 5 when sorted by population, largest to smallest.
say ($cities.sort( -*.<population> ).first: *.<population> < 5)<name>; # Khartoum-Omdurman
 
 
# Find all of the city names that contain an 'm' 
say join ', ', sort grep( {$_<name>.lc ~~ /'m'/}, @$cities )»<name>; # Dar Es Salaam, Khartoum-Omdurman, Mogadishu
```