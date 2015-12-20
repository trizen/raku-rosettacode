[1]: http://rosettacode.org/wiki/Old_Russian_measure_of_length

# [Old Russian measure of length][1]

Fairly straightfoward. Define a hash of conversion factors then apply them. Does some basic error trapping. Makes no attempt to do correct pluralization because I have no idea what the correct plurals are and little interest in researching them. Conversion factors from Wikipedia: [Obsolete Russian units of measurement](http://en.wikipedia.org/wiki/Obsolete\_Russian\_units\_of\_measurement#Length" class="extiw" title="wp:Obsolete Russian units of measurement).

```perl
convert(1, 'meter');
 
say '*' x 40, "\n";
 
convert(1, 'milia');
 
sub convert (Real $magnitude, $unit) {
     my %factor = ( 
        tochka     => 0.000254,
        liniya     => 0.00254,
        diuym      => 0.0254,
        vershok    => 0.04445,
        piad       => 0.1778,
        fut        => 0.3048,
        arshin     => 0.7112,
        sazhen     => 2.1336,
        versta     => 1066.8,
        milia      => 7467.6,
        centimeter => 0.01,
        meter      => 1.0,
        kilometer  => 1000.0,
    );
 
    die "Unknown unit $unit\n" unless %factor.exists($unit.lc);
 
    my $meters = $magnitude * %factor.delete($unit.lc);
 
    say "$magnitude $unit to:\n", '_' x 40;
 
    printf "%10s: %s\n", $_,  $meters / %factor{$_}
      for %factor.keys.sort:{ +%factor{$^_} }
}
 
```

#### Output:
```
1 meter to:
________________________________________
    tochka: 3937.007874
    liniya: 393.700787
centimeter: 100
     diuym: 39.370079
   vershok: 22.497188
      piad: 5.624297
       fut: 3.280840
    arshin: 1.406074
    sazhen: 0.468691
 kilometer: 0.001
    versta: 0.000937
     milia: 0.000134
****************************************

1 milia to:
________________________________________
    tochka: 29400000
    liniya: 2940000
centimeter: 746760
     diuym: 294000
   vershok: 168000
      piad: 42000
       fut: 24500
    arshin: 10500
     meter: 7467.6
    sazhen: 3500
 kilometer: 7.4676
    versta: 7
```