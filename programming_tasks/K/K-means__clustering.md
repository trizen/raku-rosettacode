[1]: http://rosettacode.org/wiki/K-means++_clustering

# [K-means++ clustering][1]

We use Complex numbers to represent points in the plane. We feed the algorithm with three artificially made clouds of points so we can easily see if the output makes sense.

```perl6
sub infix:«-means++»(Int $K, @data) {
    my @means = @data.pick;
    until @means == $K {
        my @cumulD2 = [\+] @data.map: -> $x {
            min @means.map: { abs($x - $_)**2 }
        }
        my $rand = rand * @cumulD2[*-1];
        @means.push: @data[
            (^@data).first: { @cumulD2[$_] > $rand }
        ];
    }
    sub cluster { @data.classify: -> $x { @means.min: { abs($_ - $x) } } }
    loop (
	my %cluster;
	1e-10 < [+] (@means Z- keys (%cluster = cluster))».abs X** 2;
        @means = %cluster.values.map( { .elems R/ [+] @$_ } )
    ) { ; }
    return @means;
}
 
my @centers = 0, 5, 3 + 2i;
my @data = flat @centers.map: { ($_ + .5 - rand + (.5 - rand) * i) xx 100 }
@data.=pick(*);
.say for 3-means++ @data;
```

#### Output:
```
5.04622376429502+0.0145269848483031i
0.0185674577571743+0.0298199687431731i
2.954898072093+2.14922298688815i
```