[1]: https://rosettacode.org/wiki/Geohash

# [Geohash][1]

### Module based



Reference: Used [this](https://www.movable-type.co.uk/scripts/geohash.html) for verification.

```perl
#20200615 Raku programming solution

use Geo::Hash;

# task example 1 : Ireland, most of England and Wales, small part of Scotland
say geo-encode(51.433718e0, -0.214126e0, 2);

# task example 2 : the umpire's chair on Center Court at Wimbledon
say geo-encode(51.433718e0, -0.214126e0, 9);

# Lake Raku is an artificial lake in Tallinn, Estonia
# https://goo.gl/maps/MEBXXhiFbN8WMo5z8
say geo-encode(59.358639e0, 24.744778e0, 4);

# Village Raku is a development committee in north-western Nepal
# https://goo.gl/maps/33s7k2h3UrHCg8Tb6
say geo-encode(29.2021188e0, 81.5324561e0, 4);
```

#### Output:
```
gc
gcpue5hp4
ud99
tv1y
```


### Roll your own



Alternately, a roll-your-own version that will work with any Real coordinate, not just floating point values, and thus can return ridiculous precision. 
The geo-decode routine returns the range in which the actual value will be found; converted here to the mid-point with the interval size. Probably better
to specify an odd precision so the error interval ends up the same for both latitude and longitude.

```perl
my @Geo32 = <0 1 2 3 4 5 6 7 8 9 b c d e f g h j k m n p q r s t u v w x y z>;

sub geo-encode ( Rat(Real) $latitude, Rat(Real) $longitude, Int $precision = 9 ) {
    my @coord = $latitude, $longitude;
    my @range = [-90, 90], [-180, 180];
    my $which = 1;
    my $value = '';
    while $value.chars < $precision * 5 {
        my $mid = @range[$which].sum / 2;
        $value ~= my $upper = +(@coord[$which] > $mid);
        @range[$which][not $upper] = $mid;
        $which = not $which;
    }
    @Geo32[$value.comb(5)».parse-base(2)].join;
}

sub geo-decode ( Str $geo ) {
     my @range = [-90, 90], [-180, 180];
     my $which = 1;
     my %Geo32 = @Geo32.antipairs;
     for %Geo32{$geo.comb}».fmt('%05b').join.comb {
         @range[$which][$_] = @range[$which].sum / 2;
         $which = not $which;
     }
     @range
}

# TESTING

for 51.433718,   -0.214126,  2, # Ireland, most of England and Wales, small part of Scotland
    51.433718,   -0.214126,  9, # the umpire's chair on Center Court at Wimbledon
    51.433718,   -0.214126, 17, # likely an individual molecule of the chair
    57.649110,   10.407440, 11, # Wikipedia test value - Råbjerg Mile in Denmark
    59.358639,   24.744778,  7, # Lake Raku in Estonia
    29.2021188, 81.5324561,  7  # Village Raku in Nepal
  -> $lat, $long, $precision {
     say "$lat, $long, $precision:\ngeo-encoded: ",
     my $enc = geo-encode $lat, $long, $precision;
     say 'geo-decoded: ', geo-decode($enc).map( {-.sum/2 ~ ' ± ' ~
          (abs(.[0]-.[1])/2).Num.fmt('%.3e')} ).join(',  ') ~ "\n";
}
```

#### Output:
```
51.433718, -0.214126, 2:
geo-encoded: gc
geo-decoded: 53.4375 ± 2.813e+00,  -5.625 ± 5.625e+00

51.433718, -0.214126, 9:
geo-encoded: gcpue5hp4
geo-decoded: 51.4337182 ± 2.146e-05,  -0.21412611 ± 2.146e-05

51.433718, -0.214126, 17:
geo-encoded: gcpue5hp4ebnf8unc
geo-decoded: 51.43371800000523 ± 2.046e-11,  -0.21412600000303 ± 2.046e-11

57.64911, 10.40744, 11:
geo-encoded: u4pruydqqvj
geo-decoded: 57.64911063 ± 6.706e-07,  10.407439694 ± 6.706e-07

59.358639, 24.744778, 7:
geo-encoded: ud99ejf
geo-decoded: 59.358444 ± 6.866e-04,  24.744644 ± 6.866e-04

29.2021188, 81.5324561, 7:
geo-encoded: tv1ypk4
geo-decoded: 29.202347 ± 6.866e-04,  81.532974 ± 6.866e-04
```
