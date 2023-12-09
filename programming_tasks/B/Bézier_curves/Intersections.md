[1]: https://rosettacode.org/wiki/Bézier_curves/Intersections

# [Bézier curves/Intersections][1]

```perl
# 20231025 Raku programming solution

class Point { has ($.x, $.y) is rw is default(0) }

class QuadSpline { has ($.c0, $.c1, $.c2) is rw is default(0) }

class QuadCurve { has QuadSpline ($.x, $.y) is rw }

class Workset { has QuadCurve ($.p, $.q) }

sub subdivideQuadSpline($q, $t) {
   my $s = 1.0 - $t;
   my ($c0,$c1,$c2) = do given $q { .c0, .c1, .c2 }; 
   my $u_c1 = $s*$c0 + $t*$c1;
   my $v_c1 = $s*$c1 + $t*$c2;
   my $u_c2 = $s*$u_c1 + $t*$v_c1;
   return ($c0, $u_c1, $u_c2), ($u_c2, $v_c1, $c2)
}

sub subdivideQuadCurve($q, $t, $u is rw, $v is rw) {
   with (subdivideQuadSpline($q.x,$t),subdivideQuadSpline($q.y,$t))».List.flat {
      $u=QuadCurve.new(x => QuadSpline.new(c0 => .[0],c1 => .[1],c2 => .[2]),
                       y => QuadSpline.new(c0 => .[6],c1 => .[7],c2 => .[8]));
      $v=QuadCurve.new(x => QuadSpline.new(c0 => .[3],c1 => .[4],c2 => .[5]),
                       y => QuadSpline.new(c0 => .[9],c1 => .[10],c2 => .[11]))
   }
}

sub seemsToBeDuplicate(@intersects, $xy, $spacing) {
   my $seemsToBeDup = False;
   for @intersects {
      $seemsToBeDup = abs(.x - $xy.x) < $spacing && abs(.y - $xy.y) < $spacing;
      last if $seemsToBeDup;
   }
   return $seemsToBeDup;
}

sub rectsOverlap($xa0, $ya0, $xa1, $ya1, $xb0, $yb0, $xb1, $yb1) {
   return $xb0 <= $xa1 && $xa0 <= $xb1 && $yb0 <= $ya1 && $ya0 <= $yb1
}

sub testIntersect($p,$q,$tol,$exclude is rw,$accept is rw,$intersect is rw) {
   my $pxmin = min($p.x.c0, $p.x.c1, $p.x.c2);
   my $pymin = min($p.y.c0, $p.y.c1, $p.y.c2);
   my $pxmax = max($p.x.c0, $p.x.c1, $p.x.c2);
   my $pymax = max($p.y.c0, $p.y.c1, $p.y.c2);
   my $qxmin = min($q.x.c0, $q.x.c1, $q.x.c2);
   my $qymin = min($q.y.c0, $q.y.c1, $q.y.c2);
   my $qxmax = max($q.x.c0, $q.x.c1, $q.x.c2);
   my $qymax = max($q.y.c0, $q.y.c1, $q.y.c2);
   ($exclude, $accept) = True, False;

   if rectsOverlap($pxmin,$pymin,$pxmax,$pymax,$qxmin,$qymin,$qxmax,$qymax) {
      $exclude = False;
      my ($xmin,$xmax) = max($pxmin, $qxmin), min($pxmax, $pxmax);
      if ($xmax < $xmin) { die "Assertion failure: $xmax < $xmin\n" }
      my ($ymin,$ymax) = max($pymin, $qymin), min($pymax, $qymax);
      if ($ymax < $ymin) { die "Assertion failure: $ymax < $ymin\n" }
      if $xmax - $xmin <= $tol and $ymax - $ymin <= $tol {
         $accept = True;
         given $intersect { (.x, .y) = 0.5 X* $xmin+$xmax, $ymin+$ymax }
      }
   }
}

sub find-intersects($p, $q, $tol, $spacing) {
   my Point @intersects;
   my @workload = Workset.new(p => $p, q => $q);

   while my $work = @workload.pop {
      my ($intersect,$exclude,$accept) = Point.new, False, False;
      testIntersect($work.p, $work.q, $tol, $exclude, $accept, $intersect); 
      if $accept {
         unless seemsToBeDuplicate(@intersects, $intersect, $spacing) {
            @intersects.push: $intersect;
         }
      } elsif not $exclude {
         my QuadCurve ($p0, $p1, $q0, $q1);
         subdivideQuadCurve($work.p, 0.5, $p0, $p1);
         subdivideQuadCurve($work.q, 0.5, $q0, $q1);
         for $p0, $p1 X $q0, $q1 { 
            @workload.push: Workset.new(p => .[0], q => .[1]) 
         }
      }
   }
   return @intersects;
}

my $p = QuadCurve.new( x => QuadSpline.new(c0 => -1.0,c1 =>  0.0,c2 => 1.0),
                       y => QuadSpline.new(c0 =>  0.0,c1 => 10.0,c2 => 0.0));
my $q = QuadCurve.new( x => QuadSpline.new(c0 =>  2.0,c1 => -8.0,c2 => 2.0),
                       y => QuadSpline.new(c0 =>  1.0,c1 =>  2.0,c2 => 3.0));
my $spacing = ( my $tol = 0.0000001 ) * 10;
.say for find-intersects($p, $q, $tol, $spacing);
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=nVdfi9tGEH8_6HcYggjSRRaW07TXOC5pUwqFQloaaKA9imyvcyI6WdJKtpbjPkle8tB-ir71C_S9n6azM7valc_JXU5wutXOzG_n_47f_dlkb7v37__q2s3k7L_P_l0VmZTw0zYvW7iCi0xCGCR9DEGiIsglNHv9XotN1hVtOI3g-uSEZX7usvUvVZGXwgmuplpyldJ7dgeAF12zs_Ie4A0dBqFft81bKVpPhCFQotISNR0guyXg3zrf5WvhcMOgRp42gqsTALhUEEhYQJpMYYLbc7MZBmhGgEYE2oQFrLfwJt-JEoIajyUbyUS0EK7nYKG6P1YpcgfyFOXhEQLiIrWgwc4jp5Y8m3vSM0MmIGbQQsTSiLZrSlaNj-J_syjGTb2I-Qj8h5snx3xAjjIu0NLsWi3HK-OWfd5eQHjcexgVdF_8AaLSxOifv5Mfc9kmmyJrGRGfoFsMKiSl2Ic9LL72Ik576DbcTH6bnsfaV3qZ4nLGy9l5FBu0G4_6CNoXDu1Lh3Z2HkVzq9zuE5R77OA-d3BP7qncV56pUweXpqiexrseIinEpXy1_VZ81yHKKmtF-BxrVjRSrFqJQewVvmSVrfLyjZ_gnhzm1_dZIQXZvdk24CG4SB1IZEsZJr0ukF4lfQTPhlPg4UOmKkNVPtU6F6u2hXwzxp2zbS6xD6jG6EZr9nInmiKrwqDPdO4revdZSmv97pe0T-9-SfvL1LjAwiMPPFuQnFZbY_H3kr-VoStDV4aOSFaZVsj2B-uvMKhirKSg3RZxIPpV0a2FqacgW61E1dqvwcWjItOhqfrLvEQP4xvhkp67Jy1Su5hFQ4eo1IhdWXZl2dWYvb_Mes2e9XdE99lvQ6993WuLXlv0-gC9ViN2ZdmVZb-B7pS5E7rP_jH00AYL9zlOusO_ajrcMLWh2TBfx7lHsYo5BjH7NmafxeyLmG2MWfeYdYpcVdkc8UvQXjcs35OAjQDtGT9jj-egE7QJ7dC9UNeQhHXxETveUutcwINvpBRNm29L2GR50TXiKYwYfy8fcBFaRdgCNVJEGUWUr4hiRdjIkSLK4KvbFPEZfUV0qyAlJ6wklSGWGWTl2khNWGqgXLnOa4uPYzp3BHODu2q8glDPGLppLWCaPIHXp3zgo8C4WdEHnWiVux715E1erieuheqeAHy9Ylc40ox5yvKars3g53ucaopttkZVzIBDt0SlbwONWtOijjg79xd5ISjztRzKDPJJta0Gd1BIh8OGNhV7iU8a6aNM9sfj7DxoefoUmrJo4Qw9LKnY83PEA5IJrImOF7CuLASOdbdeb86OQ8cOjyeQVJ28eOpJeZkwxBJEIVGpctu68vQQ0X3-eFlRP6R-Qt0ljTzMY3OW9RbmlpZk8TsJ1VboyEn62rZg8HpgwXQe-8JlBHniRlbRlMV5paesCI446PCSHmUu1gDdG5hF4-EJPjw9TXDUNvMOWjg14w5u3md4YgQCSx0YrvRgR9fCp6gGswFtcjagze6pmmfnbAB77FSzE9QCQqpj3cV0F5rSk0IEp2jU_CSRmaKQ37HVzPl3nfl5Z3_m_Q8)
