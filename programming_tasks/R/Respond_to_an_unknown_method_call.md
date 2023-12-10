[1]: https://rosettacode.org/wiki/Respond_to_an_unknown_method_call

# [Respond to an unknown method call][1]



```perl
class Farragut {
    method FALLBACK ($name, *@rest) {
        say "{self.WHAT.raku}: $name.tc() the @rest[], full speed ahead!";
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


`<a rel="nofollow" class="external text" href="https://docs.raku.org/language/typesystem#index-entry-FALLBACK_(method)">FALLBACK</a>` will be called for any method that is not defined. Since any class inherits from `Any`, there will be plenty of already defined methods. Those which are not defined can also be used as L-Values by the magic of `<a rel="nofollow" class="external text" href="https://docs.raku.org/type/Attribute#index-entry-trait_is_rw_(Attribute)-trait_is_rw">is rw</a>`.

```perl
class L-Value { 
    our $.value = 10;
    method FALLBACK($name, |c) is rw { $.value }
}

my $l = L-Value.new;
say $l.any-odd-name; # 10
$l.some-other-name = 42;
say $l.i-dont-know; # 42
```
