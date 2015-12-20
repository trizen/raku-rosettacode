[1]: http://rosettacode.org/wiki/Subleq

# [Subleq][1]

```perl6
my @memory = @*ARGS;
my $ip = 0;
while $ip >= 0 && $ip < @memory {
   my ($a, $b, $c) = @memory[$ip, $ip+1, $ip+2];
   $ip += 3;
   if $a < 0 {
       @memory[$b] = getc.ord;
   } elsif $b < 0 {
       print @memory[$a].chr;
   } else {
       if (@memory[$b] -= @memory[$a]) <= 0 {
           $ip = $c;
       } 
   }
}
```


Save as subleq.p6 then run:


#### Output:
```
perl6 subleq.p6 15 17 -1 17 -1 -1 16 1 -1 16 3 -1 15 15 0 0 -1 72 101 108 108 111 44 32 119 111 114 108 100 33 10 0
```

#### Output:
```
Hello, world!
```