[1]: https://rosettacode.org/wiki/Angles_(geometric),_normalization_and_conversion

# [Angles (geometric), normalization and conversion][1]

```raku
 
my @units =
    { code => 'd', name => 'degrees' , number =>  360 },
    { code => 'g', name => 'gradians', number =>  400 },
    { code => 'm', name => 'mills'   , number => 6400 },
    { code => 'r', name => 'radians' , number =>  tau },
;
 
my Code %cvt = (@units X @units).map: -> ($a, $b) {
    "{$a.<code>}2{$b.<code>}" => sub ($angle) {
        my $norm = $angle % $a.<number>
                 - ( $a.<number> if $angle < 0 );
        $norm * $b.<number> / $a.<number>
    }
}
 
say '     Angle Unit     ', @units».<name>».tc.fmt('%11s');
 
for -2, -1, 0, 1, 2, tau, 16, 360/tau, 360-1, 400-1, 6400-1, 1_000_000 -> $angle {
    say '';
    for @units -> $from {
        my @sub_keys = @units.map: { "{$from.<code>}2{.<code>}" };
 
        my @results = %cvt{@sub_keys}».($angle);
 
        say join ' ', $angle      .fmt('%10g'),
                      $from.<name>.fmt('%-8s'),
                      @results    .fmt('%11g');
    }
}
```

#### Output:
```
     Angle Unit         Degrees    Gradians       Mills     Radians

        -2 degrees           -2    -2.22222    -35.5556  -0.0349066
        -2 gradians        -1.8          -2         -32  -0.0314159
        -2 mills        -0.1125      -0.125          -2  -0.0019635
        -2 radians     -114.592    -127.324    -2037.18          -2

        -1 degrees           -1    -1.11111    -17.7778  -0.0174533
        -1 gradians        -0.9          -1         -16   -0.015708
        -1 mills       -0.05625     -0.0625          -1 -0.000981748
        -1 radians     -57.2958     -63.662    -1018.59          -1

         0 degrees            0           0           0           0
         0 gradians           0           0           0           0
         0 mills              0           0           0           0
         0 radians            0           0           0           0

         1 degrees            1     1.11111     17.7778   0.0174533
         1 gradians         0.9           1          16    0.015708
         1 mills        0.05625      0.0625           1 0.000981748
         1 radians      57.2958      63.662     1018.59           1

         2 degrees            2     2.22222     35.5556   0.0349066
         2 gradians         1.8           2          32   0.0314159
         2 mills         0.1125       0.125           2   0.0019635
         2 radians      114.592     127.324     2037.18           2

   6.28319 degrees      6.28319     6.98132     111.701    0.109662
   6.28319 gradians     5.65487     6.28319     100.531    0.098696
   6.28319 mills       0.353429    0.392699     6.28319   0.0061685
   6.28319 radians            0           0           0           0

        16 degrees           16     17.7778     284.444    0.279253
        16 gradians        14.4          16         256    0.251327
        16 mills            0.9           1          16    0.015708
        16 radians      196.732     218.592     3497.47     3.43363

   57.2958 degrees      57.2958      63.662     1018.59           1
   57.2958 gradians     51.5662     57.2958     916.732         0.9
   57.2958 mills        3.22289     3.58099     57.2958     0.05625
   57.2958 radians      42.8064     47.5626     761.002    0.747112

       359 degrees          359     398.889     6382.22     6.26573
       359 gradians       323.1         359        5744     5.63916
       359 mills        20.1938     22.4375         359    0.352447
       359 radians      49.1848     54.6498     874.397    0.858437

       399 degrees           39     43.3333     693.333    0.680678
       399 gradians       359.1         399        6384     6.26748
       399 mills        22.4438     24.9375         399    0.391717
       399 radians      181.016     201.129     3218.06     3.15933

      6399 degrees          279         310        4960     4.86947
      6399 gradians       359.1         399        6384     6.26748
      6399 mills        359.944     399.938        6399      6.2822
      6399 radians      155.693     172.992     2767.88     2.71736

   1000000 degrees          280     311.111     4977.78     4.88692
   1000000 gradians           0           0           0           0
   1000000 mills             90         100        1600      1.5708
   1000000 radians      339.513     377.237     6035.79     5.92562
```


Alternately, implemented as a series of postfix operators:



*(Much of the complexity is due to the requirement that negative angles must normalize to a negative number.)*

```raku
sub postfix:<t>( $t ) { my $a = $t % 1 * τ;           $t >=0 ?? $a !! $a - τ }
sub postfix:<d>( $d ) { my $a = $d % 360 * τ / 360;   $d >=0 ?? $a !! $a - τ }
sub postfix:<g>( $g ) { my $a = $g % 400 * τ / 400;   $g >=0 ?? $a !! $a - τ }
sub postfix:<m>( $m ) { my $a = $m % 6400 * τ / 6400; $m >=0 ?? $a !! $a - τ }
sub postfix:<r>( $r ) { my $a = $r % τ;               $r >=0 ?? $a !! $a - τ }
 
sub postfix:«->t» ($angle) { my $a = $angle / τ;        ($angle < 0 and $a == -1)    ?? -0 !! $a }
sub postfix:«->d» ($angle) { my $a = $angle / τ * 360;  ($angle < 0 and $a == -360)  ?? -0 !! $a }
sub postfix:«->g» ($angle) { my $a = $angle / τ * 400;  ($angle < 0 and $a == -400)  ?? -0 !! $a }
sub postfix:«->m» ($angle) { my $a = $angle / τ * 6400; ($angle < 0 and $a == -6400) ?? -0 !! $a }
sub postfix:«->r» ($angle) { my $a = $angle;            ($angle < 0 and $a == -τ)    ?? -0 !! $a }
 
for <-2 -1 0 1 2 6.2831853 16 57.2957795 359 399 6399 1000000> -> $a {
    say '';
    say '  Quantity  Unit      ', <turns degrees gradians mils radians>.fmt('%15s');
    for 'turns', &postfix:«t», 'degrees', &postfix:«d», 'gradians', &postfix:«g»,
        'mils',  &postfix:«m», 'radians', &postfix:«r»
      -> $unit, &f {
            printf "%10s %-10s %15s %15s %15s %15s %15s\n", $a, $unit,
            |($a.&f->t, $a.&f->d, $a.&f->g, $a.&f->m, $a.&f->r)».round(.00000001);
    }
}
```

#### Output:
```
 Quantity  Unit                turns         degrees        gradians            mils         radians
        -2 turns                    0               0               0               0               0
        -2 degrees        -0.00555556              -2     -2.22222222    -35.55555556     -0.03490659
        -2 gradians            -0.005            -1.8              -2             -32     -0.03141593
        -2 mils             -0.000313         -0.1125          -0.125              -2      -0.0019635
        -2 radians        -0.31830989   -114.59155903   -127.32395447  -2037.18327158              -2

  Quantity  Unit                turns         degrees        gradians            mils         radians
        -1 turns                    0               0               0               0               0
        -1 degrees        -0.00277778              -1     -1.11111111    -17.77777778     -0.01745329
        -1 gradians           -0.0025            -0.9              -1             -16     -0.01570796
        -1 mils             -0.000156        -0.05625         -0.0625              -1     -0.00098175
        -1 radians        -0.15915494    -57.29577951    -63.66197724  -1018.59163579              -1

  Quantity  Unit                turns         degrees        gradians            mils         radians
         0 turns                    0               0               0               0               0
         0 degrees                  0               0               0               0               0
         0 gradians                 0               0               0               0               0
         0 mils                     0               0               0               0               0
         0 radians                  0               0               0               0               0

  Quantity  Unit                turns         degrees        gradians            mils         radians
         1 turns                    0               0               0               0               0
         1 degrees         0.00277778               1      1.11111111     17.77777778      0.01745329
         1 gradians            0.0025             0.9               1              16      0.01570796
         1 mils              0.000156         0.05625          0.0625               1      0.00098175
         1 radians         0.15915494     57.29577951     63.66197724   1018.59163579               1

  Quantity  Unit                turns         degrees        gradians            mils         radians
         2 turns                    0               0               0               0               0
         2 degrees         0.00555556               2      2.22222222     35.55555556      0.03490659
         2 gradians             0.005             1.8               2              32      0.03141593
         2 mils              0.000313          0.1125           0.125               2       0.0019635
         2 radians         0.31830989    114.59155903    127.32395447   2037.18327158               2

  Quantity  Unit                turns         degrees        gradians            mils         radians
 6.2831853 turns            0.2831853      101.946708       113.27412      1812.38592      1.77930572
 6.2831853 degrees         0.01745329       6.2831853        6.981317      111.701072      0.10966227
 6.2831853 gradians        0.01570796      5.65486677       6.2831853     100.5309648      0.09869604
 6.2831853 mils            0.00098175      0.35342917      0.39269908       6.2831853       0.0061685
 6.2831853 radians                  1    359.99999959    399.99999954   6399.99999269       6.2831853

  Quantity  Unit                turns         degrees        gradians            mils         radians
        16 turns                    0               0               0               0               0
        16 degrees         0.04444444              16     17.77777778    284.44444444      0.27925268
        16 gradians              0.04            14.4              16             256      0.25132741
        16 mils                0.0025             0.9               1              16      0.01570796
        16 radians         0.54647909    196.73247221    218.59163579   3497.46617261      3.43362939

  Quantity  Unit                turns         degrees        gradians            mils         radians
57.2957795 turns            0.2957795       106.48062        118.3118       1892.9888      1.85843741
57.2957795 degrees         0.15915494      57.2957795     63.66197722   1018.59163556               1
57.2957795 gradians        0.14323945     51.56620155      57.2957795      916.732472             0.9
57.2957795 mils            0.00895247       3.2228876      3.58098622      57.2957795         0.05625
57.2957795 radians         0.11890653     42.80634926     47.56261029    761.00176466      0.74711174

  Quantity  Unit                turns         degrees        gradians            mils         radians
       359 turns                    0               0               0               0               0
       359 degrees         0.99722222             359    398.88888889   6382.22222222      6.26573201
       359 gradians            0.8975           323.1             359            5744      5.63915881
       359 mils              0.056094        20.19375         22.4375             359      0.35244743
       359 radians         0.13662457      49.1848452       54.649828    874.39724794      0.85843749

  Quantity  Unit                turns         degrees        gradians            mils         radians
       399 turns                    0               0               0               0               0
       399 degrees         0.10833333              39     43.33333333    693.33333333      0.68067841
       399 gradians            0.9975           359.1             399            6384      6.26747734
       399 mils              0.062344        22.44375         24.9375             399      0.39171733
       399 radians         0.50282229    181.01602572    201.12891747   3218.06267946      3.15932565

  Quantity  Unit                turns         degrees        gradians            mils         radians
      6399 turns                    0               0               0               0               0
      6399 degrees              0.775             279             310            4960      4.86946861
      6399 gradians            0.9975           359.1             399            6384      6.26747734
      6399 mils              0.999844       359.94375        399.9375            6399      6.28220356
      6399 radians         0.43248085    155.69310421    172.99233802   2767.87740825      2.71735729

  Quantity  Unit                turns         degrees        gradians            mils         radians
   1000000 turns                    0               0               0               0               0
   1000000 degrees         0.77777778             280    311.11111111   4977.77777778      4.88692191
   1000000 gradians                 0               0               0               0               0
   1000000 mils                  0.25              90             100            1600      1.57079633
   1000000 radians          0.9430919    339.51308233    377.23675814   6035.78813022      5.92562114
```
