[1]: https://rosettacode.org/wiki/Euclidean_rhythm

# [Euclidean rhythm][1]

```perl
# 20240208 Raku programming solution

say sub ($k is copy, $n is copy) {
   my @s = [ [1] xx $k ].append: [0] xx ($n - $k); 
   my $z = my $d = $n - $k;
   ($k, $n) = ($d, $k).minmax.bounds;

   while $z > 0 || $k > 1 {
      ^$k .map: { @s[$_].append: @s.splice(*-1) } 
      ($z, $d) = ($z, $n) X- $k;
      ($k, $n) = ($d, $k).minmax.bounds;
   }
   return [~] @s>>.List.flat;
}(5, 13);
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jVBBboMwEFSvvGIOPpgKrKCqUhVUlAf0AZUsGjmBqCjgWBhUIKEf6SWH9lN9TdeFpNf64F3vznhG8_FZq317Pn-1zS58-L5ZW9XDthtwtkdhsT2YPgDTl97H0QNQ9VhZPEJCRim6DoROhTIm19kScvE740QLaePHmDlsII6rGdV5G7sdqTkVn8acZYEjiarQlerE5tDqzMaeg729FmXufkmwwOnkVBNEkyU6L_QWlTJLHMmeZOs_SysrrCmLbc5vw8jHiJnC2UBq2SQ8TB6er7b-54xQo7vqvGlrDfmeklySiKfCNmJXqib2Rn4fILrz4ynnOe5L7D8)
