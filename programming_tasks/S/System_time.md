[1]: https://rosettacode.org/wiki/System_time

# [System time][1]

```raku
say DateTime.now;
dd DateTime.now.Instant;
```

#### Output:
```
 2015-11-02T13:34:09+01:00
 Instant $var = Instant.new(<2159577143734/1493>)
```