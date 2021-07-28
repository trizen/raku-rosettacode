[1]: https://rosettacode.org/wiki/Air_mass

# [Air mass][1]

```perl
constant DEG = pi/180;    # degrees to radians
constant RE  = 6371000;   # Earth radius in meters
constant dd  = 0.001;     # integrate in this fraction of the distance already covered
constant FIN = 10000000;  # integrate only to a height of 10000km, effectively infinity
 
#| Density of air as a function of height above sea level
sub rho ( \a ) { exp( -a / 8500 ) }
 
sub height ( \a, \z, \d ) {
    # a = altitude of observer
    # z = zenith angle (in degrees)
    # d = distance along line of sight
    my \AA = RE + a;
    sqrt( AA² + d² - 2*d*AA*cos((180-z)*DEG) ) - AA;
}
 
#| Integrates density along the line of sight
sub column_density ( \a, \z ) {
    my $sum = 0;
    my $d   = 0;
    while $d < FIN {
        my \delta = max(dd, (dd)*$d);  # Adaptive step size to avoid it taking forever
        $sum += rho(height(a, z, $d + delta/2))*delta;
        $d   += delta;
    }
    $sum;
}
 
sub airmass ( \a, \z ) {
    column_density( a, z ) /
    column_density( a, 0 )
}
 
say 'Angle     0 m              13700 m';
say '------------------------------------';
say join "\n", (0, 5 ... 90).hyper(:3batch).map: -> \z {
    sprintf "%2d      %11.8f      %11.8f", z, airmass(    0, z), airmass(13700, z);
}
```

#### Output:
```
Angle     0 m              13700 m
------------------------------------
 0       1.00000000       1.00000000
 5       1.00380963       1.00380965
10       1.01538466       1.01538475
15       1.03517744       1.03517765
20       1.06399053       1.06399093
25       1.10305937       1.10306005
30       1.15418974       1.15419083
35       1.21998076       1.21998246
40       1.30418931       1.30419190
45       1.41234169       1.41234567
50       1.55280404       1.55281025
55       1.73875921       1.73876915
60       1.99212000       1.99213665
65       2.35199740       2.35202722
70       2.89531368       2.89537287
75       3.79582352       3.79596149
80       5.53885809       5.53928113
85      10.07896219      10.08115981
90      34.32981136      34.36666557
```
