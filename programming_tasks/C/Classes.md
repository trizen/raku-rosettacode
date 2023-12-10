[1]: https://rosettacode.org/wiki/Classes

# [Classes][1]



```perl
class Camel { has Int $.humps = 1; }

my Camel $a .= new;
say $a.humps;  # Automatically generated accessor method.

my Camel $b .= new: humps => 2;
say $b.humps;
```


A more complex example:

```perl
class Butterfly {
    has Int $!age;   # With the ! twigil, no public accessor method is generated
    has Str $.name;
    has Str $.color;
    has Bool $.wings;

    submethod BUILD(:$!name = 'Camelia', :$!age = 2, :$!color = 'pink') {
        # BUILD is called by bless. Its primary use is to to control
        # object initialization.
        $!wings = $!age > 1;
    }

    method flap() {
        say ($.wings
          ?? 'Watch out for that hurricane!'
          !! 'No wings to flap.');
    }
}

my Butterfly $a .= new: age => 5;
say "Name: {$a.name}, Color: {$a.color}";
$a.flap;

my Butterfly $b .= new(name => 'Osgood', age => 4);
say "Name: {$b.name}, Color: {$b.color}";
$b.flap;
```
