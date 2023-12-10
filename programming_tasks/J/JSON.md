[1]: https://rosettacode.org/wiki/JSON

# [JSON][1]





Using [JSON::Tiny](https://github.com/moritz/json/)

```perl
use JSON::Tiny;

say from-json '{ "foo": 1, "bar": [10, "apples"] }';
say to-json   %( blue => [1,2], ocean => "water" );
```

#### Output:
```
{bar => [10 apples], foo => 1}
{ "blue" : [ 1, 2 ], "ocean" : "water" }
```
