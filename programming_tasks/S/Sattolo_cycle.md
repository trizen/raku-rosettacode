[1]: https://rosettacode.org/wiki/Sattolo_cycle

# [Sattolo cycle][1]





This modifies the array passed as argument, in-place.

```perl
sub sattolo-cycle (@array) {
    for reverse 1 .. @array.end -> $i {
        my $j = (^$i).pick;
        @array[$j, $i] = @array[$i, $j];
    }
}

my @a = flat 'A' .. 'Z', 'a' .. 'z';

say @a;
sattolo-cycle(@a);
say @a;
```

#### Output:
```
[A B C D E F G H I J K L M N O P Q R S T U V W X Y Z a b c d e f g h i j k l m n o p q r s t u v w x y z]
[r G w g W Z D X M f Q A c i H Y J F s z m v x P b U j n q I N e O L o C d u a K S V l y R T B k t h p E]
```
