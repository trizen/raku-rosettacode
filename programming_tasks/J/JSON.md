[1]: http://rosettacode.org/wiki/JSON

# [JSON][1]

Using [JSON::Tiny](http://github.com/moritz/json/)

```perl
use JSON::Tiny;
 
my $data = from-json('{ "foo": 1, "bar": [10, "apples"] }');
 
my $sample = { blue => [1,2], ocean => "water" };
my $json_string = to-json($sample);
```