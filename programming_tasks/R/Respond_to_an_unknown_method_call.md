[1]: https://rosettacode.org/wiki/Respond_to_an_unknown_method_call

# [Respond to an unknown method call][1]

```raku
class Farragut {
    method FALLBACK ($name, *@rest) {
        say "{self.WHAT.perl}: $name.tc() the @rest[], full speed ahead!";
    }
}
 
class Sparrow is Farragut { }
 
Farragut.damn: 'torpedoes';
Sparrow.hoist: <Jolly Roger mateys>;
```

#### Output:
```
Farragut: Damn the torpedoes, full speed ahead!
Sparrow: Hoist the Jolly Roger mateys, full speed ahead!
```


`<a rel="nofollow" class="external text" href="http://design.perl6.org/S12.html#FALLBACK_methods">FALLBACK</a>` will be called for any method that is not defined. Since any class inherits from `Any`, there will be plenty of already defined methods. Those which are not defined can also be used as L-Values by the magic of `<a rel="nofollow" class="external text" href="http://design.perl6.org/S12.html#Lvalue_methods">is rw</a>`.

```raku
class L-Value { 
    our $.value = 10;
    method FALLBACK($name, |c) is rw { $.value }
}
 
my $l = L-Value.new;
say $l.any-odd-name; # 10
$l.some-other-name = 42;
say $l.i-dont-know; # 42
```