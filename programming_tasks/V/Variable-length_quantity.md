[1]: https://rosettacode.org/wiki/Variable-length_quantity

# [Variable-length quantity][1]


vlq\_encode() returns a string of encoded octets. vlq\_decode() takes a string and returns a decimal number.

```perl
sub vlq_encode ($number is copy) {
    my @vlq = (127 +& $number).fmt("%02X");
    $number +>= 7;
    while ($number) {
       @vlq.push: (128 +| (127 +& $number)).fmt("%02X");
       $number +>= 7; 
    }
    @vlq.reverse.join: ':';
}

sub vlq_decode ($string) {
    sum $string.split(':').reverse.map: {(:16($_) +& 127) +< (7 × $++)}
}

#test encoding and decoding
for (
    0,   0xa,   123,   254,   255,   256,
    257, 65534, 65535, 65536, 65537, 0x1fffff,
    0x200000
 ) -> $testcase {
    printf "%8s %12s %8s\n", $testcase,
      my $encoded = vlq_encode($testcase),
      vlq_decode($encoded);
}
```


Output:


```
       0           00        0
      10           0A       10
     123           7B      123
     254        81:7E      254
     255        81:7F      255
     256        82:00      256
     257        82:01      257
   65534     83:FF:7E    65534
   65535     83:FF:7F    65535
   65536     84:80:00    65536
   65537     84:80:01    65537
 2097151     FF:FF:7F  2097151
 2097152  81:80:80:00  2097152
```
