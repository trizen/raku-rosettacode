[1]: https://rosettacode.org/wiki/Multiton

# [Multiton][1]

Tried to translate [the C# example](https://wikipedia.org/wiki/Multiton_pattern#Implementations) at WP but not sure if my  interpretation/implementation is correct

```perl
# 20211001 Raku programming solution 

enum MultitonType < Gold Silver Bronze >;

class Multiton { 

   my %instances = MultitonType.keys Z=> $ ⚛= 1 xx * ;

   has $.type is rw; 

   method TWEAK { $.type = 'Nothing' unless cas(%instances{$.type}, 1, 0) }
}

race for ^10 -> $i {
   Thread.start(
      sub {
#         sleep roll(^2);
         my $obj = Multiton.new: type => MultitonType.roll;
         say "Thread ", $i, " has got ", $obj.type;
      }
   );
}
```

#### Output:
```
Thread 5 has got Bronze
Thread 9 has got Gold
Thread 7 has got Nothing
Thread 8 has got Nothing
Thread 3 has got Nothing
Thread 2 has got Nothing
Thread 1 has got Nothing
Thread 0 has got Silver
Thread 4 has got Nothing
Thread 6 has got Nothing
```
