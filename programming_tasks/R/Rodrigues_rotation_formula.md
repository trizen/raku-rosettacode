[1]: https://rosettacode.org/wiki/Rodrigues’_rotation_formula

# [Rodrigues’ rotation formula][1]

```perl
sub infix:<⋅> { [+] @^a »×« @^b }
sub norm      (@v) { sqrt @v⋅@v }
sub normalize (@v) { @v X/ @v.&norm }
sub getAngle  (@v1,@v2) { 180/π × acos (@v1⋅@v2) / (@v1.&norm × @v2.&norm) }

sub crossProduct ( @v1, @v2 ) {
    my \a = <1 2 0>; my \b = <2 0 1>;
    (@v1[a] »×« @v2[b]) »-« (@v1[b] »×« @v2[a])
}

sub aRotate ( @p, @v, $a ) {
    my \ca = cos $a/180×π;
    my \sa = sin $a/180×π;
    my \t = 1 - ca;
    my (\x,\y,\z) = @v;
    map { @p⋅$_ },
        [   ca + x×x×t, x×y×t -  z×sa, x×z×t +  y×sa],
        [x×y×t +  z×sa,    ca + y×y×t, y×z×t -  x×sa],
        [z×x×t -  y×sa, z×y×t +  x×sa,    ca + z×z×t]
}

my @v1 = [5,-6,  4];
my @v2 = [8, 5,-30];
say join ' ', aRotate @v1, normalize(crossProduct @v1, @v2), getAngle @v1, @v2;
```

#### Output:
```
2.232221073308229 1.3951381708176411 -8.370829024905852
```


Alternately, differing mostly in style:

```perl
sub infix:<•> { sum @^v1 Z× @^v2 } # dot product

sub infix:<❌> (@v1, @v2) {         # cross product
    my \a = <1 2 0>; my \b = <2 0 1>;
    @v1[a] »×« @v2[b] »-« @v1[b] »×« @v2[a]
}

sub norm (*@v) { sqrt @v • @v }

sub normal (*@v) { @v X/ @v.&norm }

sub angle-between (@v1, @v2) { acos( (@v1 • @v2) / (@v1.&norm × @v2.&norm) ) }

sub infix:<⁢> is equiv(&infix:<×>) { $^a × $^b } # invisible times

sub postfix:<°> (\d) { d × τ / 360 } # degrees to radians

sub rodrigues-rotate( @point, @axis, $θ ) {
    my ( \cos𝜃, \sin𝜃 ) = cis($θ).reals;
    my ( \𝑥, \𝑦, \𝑧 )   = @axis;
    my \𝑡 = 1 - cos𝜃;

    map @point • *, [
       [ 𝑥²⁢𝑡 + cos𝜃, 𝑦⁢𝑥⁢𝑡 - 𝑧⁢sin𝜃, 𝑧⁢𝑥⁢𝑡 + 𝑦⁢sin𝜃 ],
       [ 𝑥⁢𝑦⁢𝑡 + 𝑧⁢sin𝜃, 𝑦²⁢𝑡 + cos𝜃, 𝑧⁢𝑦⁢𝑡 - 𝑥⁢sin𝜃 ],
       [ 𝑥⁢𝑧⁢𝑡 - 𝑦⁢sin𝜃, 𝑦⁢𝑧⁢𝑡 + 𝑥⁢sin𝜃, 𝑧²⁢𝑡 + cos𝜃 ]
    ]
}

sub point-vector (@point, @vector) {
    rodrigues-rotate @point, normal(@point ❌ @vector), angle-between @point, @vector
}

put qq:to/TESTING/;
Task example - Point and composite axis / angle:
{ point-vector [5, -6, 4], [8, 5, -30] }

Perhaps more useful, (when calculating a range of motion for a robot appendage,
for example), feeding a point, axis of rotation and rotation angle separately;
since theoretically, the point vector and axis of rotation should be constant:

{
(0, 10, 20 ... 180).map( { # in degrees
    sprintf "Rotated %3d°: %.13f, %.13f, %.13f", $_,
    rodrigues-rotate [5, -6, 4], ([5, -6, 4] ❌ [8, 5, -30]).&normal, .°
}).join: "\n"
}
TESTING
```

#### Output:
```
Task example - Point and composite axis / angle:
2.232221073308228 1.3951381708176427 -8.370829024905852

Perhaps more useful, (when calculating a range of motion for a robot appendage,
for example), feeding a point, axis of rotation and rotation angle directly;
since theoretically, the point vector and axis of rotation should be constant:

Rotated   0°: 5.0000000000000, -6.0000000000000, 4.0000000000000
Rotated  10°: 5.7240554466526, -6.0975296976939, 2.6561853906284
Rotated  20°: 6.2741883650704, -6.0097890410223, 1.2316639322573
Rotated  30°: 6.6336832449081, -5.7394439854392, -0.2302810114435
Rotated  40°: 6.7916170161550, -5.2947088286573, -1.6852289831393
Rotated  50°: 6.7431909410900, -4.6890966233686, -3.0889721249495
Rotated  60°: 6.4898764214992, -3.9410085899762, -4.3988584118384
Rotated  70°: 6.0393702908772, -3.0731750048240, -5.5750876118134
Rotated  80°: 5.4053609500356, -2.1119645522518, -6.5819205958338
Rotated  90°: 4.6071124519719, -1.0865831254651, -7.3887652531624
Rotated 100°: 3.6688791733663, -0.0281864202486, -7.9711060171693
Rotated 110°: 2.6191688576205, 1.0310667150840, -8.3112487584187
Rotated 120°: 1.4898764214993, 2.0589914100238, -8.3988584118384
Rotated 130°: 0.3153148442246, 3.0243546928699, -8.2312730024418
Rotated 140°: -0.8688274150348, 3.8978244887705, -7.8135845280911
Rotated 150°: -2.0265707929363, 4.6528608599741, -7.1584842417190
Rotated 160°: -3.1227378427887, 5.2665224084086, -6.2858770340300
Rotated 170°: -4.1240220834695, 5.7201633384526, -5.2222766334692
Rotated 180°: -5.0000000000000, 6.0000000000000, -4.0000000000000
```
