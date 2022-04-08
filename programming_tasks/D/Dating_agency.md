[1]: https://rosettacode.org/wiki/Dating_agency

# [Dating agency][1]

Welcome to the **Arbitrary and Capricious Dating Agency**, (We don't use zodiacs, but we're just as arbitrary!)

```perl
use Digest::SHA1::Native;
 
my %ladies = < Alice Beth Cecilia Donna Eunice Fran Genevieve Holly Irene Josephine Kathlene Loralie Margaret
               Nancy Odelle Pamela Quinci Rhonda Stephanie Theresa Ursula Victoria Wren Yasmine Zoey >.map: -> $name {
    $name => {
        loves   => :16(sha1-hex($name).substr(0,4)),
        lovable => :16(sha1-hex($name).substr(*-4))
    }
}
 
my %sailors = < Ahab Brutus Popeye >.map: -> $name {
    $name => {
        loves   => :16(sha1-hex($name).substr(0,4)),
        lovable => :16(sha1-hex($name).substr(*-4))
    }
}
 
for %sailors.sort( *.key ) -> $sailor {
    printf "%6s will like: ", $sailor.key;
    say join ', ', my @likes = %ladies.sort( { abs $sailor.value.<loves> - .value.<loves> } ).head(10)».key;
    print '     Is liked by: ';
    say join ', ',  my @liked = %ladies.sort( { abs $sailor.value.<lovable> - .value.<lovable> } ).head(10)».key;
    my %matches;
    for @liked.reverse Z, (1..10) { %matches{.[0]} += .[1] };
    for @likes.reverse Z, (1..10) { %matches{.[0]} += .[1] };
    say 'Best match(s): ' ~ %matches.grep(*.value == %matches.values.max)».key.sort.join(', ');
    say '';
}
```

#### Output:
```
  Ahab will like: Eunice, Ursula, Irene, Holly, Fran, Genevieve, Zoey, Donna, Cecilia, Alice
     Is liked by: Nancy, Theresa, Victoria, Genevieve, Alice, Rhonda, Kathlene, Odelle, Eunice, Pamela
Best match(s): Eunice, Genevieve

Brutus will like: Beth, Nancy, Quinci, Odelle, Josephine, Kathlene, Theresa, Rhonda, Wren, Margaret
     Is liked by: Yasmine, Margaret, Loralie, Holly, Wren, Fran, Quinci, Eunice, Alice, Victoria
Best match(s): Quinci

Popeye will like: Loralie, Theresa, Stephanie, Yasmine, Alice, Cecilia, Donna, Zoey, Nancy, Genevieve
     Is liked by: Loralie, Margaret, Holly, Wren, Yasmine, Fran, Quinci, Eunice, Alice, Victoria
Best match(s): Loralie
```
