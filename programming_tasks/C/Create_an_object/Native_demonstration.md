[1]: http://rosettacode.org/wiki/Create_an_object/Native_demonstration

# [Create an object/Native demonstration][1]

Here we use delegation to handle all the normal hash methods that we don't need to override to define our new class.

```perl
class FixedHash {
        has $.hash handles *;
        method new(*@args) { self.bless: *, hash => Hash.new: @args }
        method AT-KEY(FixedHash:D: $key is copy) is rw {
                $!hash.EXISTS-KEY($key) ?? $!hash.AT-KEY($key) !! Failure.new(q{can't store value for unknown key});
        }
        method DELETE-KEY($key) { $!hash.{$key} = Nil }
}
 
# Testing
my $fh = FixedHash.new: "a" => 1, "b" => 2;
say $fh<a b>;   # 1 2
$fh<b>:delete;
say $fh<a b>;   # 1 Nil
$fh<b> = 42;
say $fh<a b>;   # 1 42
say $fh<c>;     # Nil
$fh<c> = 43;    # error

```

#### Output:
```
(1 2)
(1 (Any))
(1 42)
can't store value for unknown key
  in block <unit> at native-demonstration.p6:17

Actually thrown at:
  in block <unit> at native-demonstration.p6:17
```


By defining [FALLBACK](http://design.perl6.org/S12.html#FALLBACK\_methods) any class can handle undefined method calls. Since any class inherits plenty of methods from <tt>Any</tt> our magic object will be more of a novice conjurer then a master wizard proper.

```perl
class Magic {
        has %.hash;
        multi method FALLBACK($name, |c) is rw { # this will eat any extra parameters
                %.hash{$name}
        }
 
        multi method FALLBACK($name) is rw {
                %.hash{$name}
        }
}
 
my $magic = Magic.new;
$magic.foo = 10;
say $magic.foo;
$magic.defined = False; # error
```

#### Output:
```
10
Cannot modify an immutable Bool
  in block <unit> at native-demonstration.p6:15
```