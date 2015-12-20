[1]: http://rosettacode.org/wiki/Random_number_generator_(device)

# [Random number generator (device)][1]

A lazy list of random numbers:

```perl6
my $UR = open("/dev/urandom", :bin) or die "Can't open /dev/urandom: $!";
my @random-spigot := gather loop { take $UR.read(1024).unpack("L*") }
 
.say for @random-spigot[^10];
```

#### Output:
```
1431009271
1702240522
670020272
588612037
1864913839
2155430433
1690056587
385405103
2366495746
692037942
```