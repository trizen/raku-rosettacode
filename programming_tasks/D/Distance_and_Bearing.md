[1]: https://rosettacode.org/wiki/Distance_and_Bearing

# [Distance and Bearing][1]

```perl
use Text::CSV;

constant $EARTH_RADIUS_IN_NAUTICAL_MILES = 6372.8 / 1.852;

sub degrees   ( Real $rad ) { $rad / tau * 360 }
sub radians   ( Real $deg ) { $deg * tau / 360 }
sub haversine ( Real $x   ) { ($x / 2).sin.²   }
sub arc_haver ( Real $x   ) { $x.sqrt.asin * 2 }

sub great_circle_distance ( \φ1, \λ1, \φ2, \λ2 ) {
    # https://en.wikipedia.org/wiki/Haversine_formula
    #   latitude (represented by φ) and longitude (represented by λ)
    #   hav(θ) = hav(φ₂ − φ₁) + cos(φ₁) cos(φ₂) hav(λ₂ − λ₁)
    arc_haver(
        haversine(φ2 - φ1)
      + haversine(λ2 - λ1) * cos(φ1) * cos(φ2)
    );
}

sub great_circle_bearing ( \φ1, \λ1, \φ2, \λ2 ) {
    atan2(                          φ2.cos * (λ2 - λ1).sin,
        φ1.cos * φ2.sin - φ1.sin * φ2.cos * (λ2 - λ1).cos,
    );
}

sub distance_and_bearing ( $lat1, $lon1, $lat2, $lon2 ) {
    my @ll = map &radians, $lat1, $lon1, $lat2, $lon2;

    my $dist  = great_circle_distance(|@ll);
    my $theta = great_circle_bearing( |@ll);

    return  $dist * $EARTH_RADIUS_IN_NAUTICAL_MILES,
            degrees( $theta + (tau if $theta < 0) )
}

sub find_nearest_airports ( $latitude, $longitude, $csv_path ) {
    my &d_and_b_from_here = &distance_and_bearing.assuming($latitude, $longitude);

    my @airports = csv(
        in => $csv_path,
        headers => [<ID Name City Country IATA ICAO Latitude Longitude>],
    );

    for @airports -> %row {
        %row<Distance Bearing> = d_and_b_from_here( +%row<Latitude>, +%row<Longitude> );
    }

    return @airports.sort(*.<Distance>);
}

sub MAIN ( $lat = 51.514669, $long = 2.198581, $wanted = 20, $csv = 'airports.dat' ) {
    printf "%7s\t%7s\t%-7s\t%-15s\t%s\n", <Dist Bear ICAO Country Name>;

    for find_nearest_airports($lat, $long, $csv).head($wanted) {
        printf "%7.1f\t    %03d\t%-7s\t%-15s\t%s\n",
            .<Distance Bearing ICAO Country Name>;
    }
}
```

#### Output:
```
   Dist    Bear ICAO    Country         Name
   30.7     146 EBFN    Belgium         Koksijde Air Base
   31.3     127 EBOS    Belgium         Ostend-Bruges International Airport
   33.6     252 EGMH    United Kingdom  Kent International Airport
   34.4     195 LFAC    France          Calais-Dunkerque Airport
   42.6     105 EBKW    Belgium         Westkapelle heliport
   51.6     240 EGMK    United Kingdom  Lympne Airport
   52.8     114 EBUL    Belgium         Ursel Air Base
   56.2     274 EGMC    United Kingdom  Southend Airport
   56.4     162 LFQT    France          Merville-Calonne Airport
   56.5     137 EBKT    Belgium         Wevelgem Airport
   57.3     089 EHMZ    Netherlands     Midden-Zeeland Airport
   58.0     235 EGMD    United Kingdom  Lydd Airport
   59.0     309 EGUW    United Kingdom  RAF Wattisham
   59.3     339 EGSM    United Kingdom  Beccles Airport
   59.7     146 LFQO    France          Lille/Marcq-en-Baroeul Airport
   62.2     250 EGKH    United Kingdom  Lashenden (Headcorn) Airfield
   63.7     200 LFAT    France          Le Touquet-Côte d'Opale Airport
   64.2     261 EGTO    United Kingdom  Rochester Airport
   66.3     149 LFQQ    France          Lille-Lesquin Airport
   68.4     271 EGMT    United Kingdom  Thurrock Airfield
```
