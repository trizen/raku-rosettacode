[1]: https://rosettacode.org/wiki/Check_if_two_polygons_overlap

# [Check if two polygons overlap][1]

```perl
# 20230810 Raku programming solution

class Vector2 { has ( $.x, $.y );
   method dot ( \other ) { self.x * other.x + self.y * other.y }
}; 

class Projection { has ( $.min, $.max ) };

sub getAxes ( \poly ) {
   return poly.append(poly[0]).rotor(2=>-1).map: -> (\vertex1,\vertex2) {
      my \vector1 = Vector2.new: x => vertex1[0], y => vertex1[1];
      my \vector2 = Vector2.new: x => vertex2[0], y => vertex2[1];
      my \edge    = Vector2.new: x => vector1.x - vector2.x, 
                                 y => vector1.y - vector2.y;
      $_ = Vector2.new: x => -edge.y, y => edge.x
   }
}

sub projectOntoAxis ( \poly, \axis ) {
   my \vertex0 = poly[0];
   my \vector0 = Vector2.new: x => vertex0[0], y => vertex0[1];
   my $max     = my $min = axis.dot: vector0;
   for poly -> \vertex {
      my \vector = Vector2.new: x => vertex[0], y => vertex[1];
      given axis.dot: vector { when $_ < $min { $min = $_ } 
                               when $_ > $max { $max = $_ } }
   }
   return Projection.new: min => $min, max => $max
}

sub projectionsOverlap ( \proj1, \proj2 ) {
   return ! ( proj1.max < proj2.min or proj2.max < proj1.min )
}

sub polygonsOverlap( \poly1, \poly2 ) {
   my (\axes1,\axes2) := (poly1,poly2).map: { getAxes $_ };
   for (axes1, axes2) -> \axes {
     for axes -> \axis {
         my (\proj1,\proj2) := (poly1,poly2).map: { projectOntoAxis $_, axis }
         return False unless projectionsOverlap(proj1, proj2) 
      }
   }
   return True
}

my \poly1 = [ <0 0>, <0 2>, <1 4>, <2 2>, <2 0> ];
my \poly2 = [ <4 0>, <4 2>, <5 4>, <6 2>, <6 0> ];
my \poly3 = [ <1 0>, <1 2>, <5 4>, <9 2>, <9 0> ];

say "poly1 = ", poly1;
say "poly2 = ", poly2;
say "poly3 = ", poly3;
say();
say "poly1 and poly2 overlap? ", polygonsOverlap(poly1, poly2);
say "poly1 and poly3 overlap? ", polygonsOverlap(poly1, poly3);
say "poly2 and poly3 overlap? ", polygonsOverlap(poly2, poly3);
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jVXNbtpAED5W8lNMIw52ayx7oVGDwVUuvVXpoeolRJEbFnBLbOSfxBbiSXrJoX2pPk1ndtY_MZSGA7s7O998M_PNws9fafijeHr6XeTL4fs_rz7dbcIsg6_yLk9SATtYhxmYMHBKG78qsHwDAO5lvk4WsEhyvJsn-VqmYKFzJjdLp4Q3oEy4e8umqjFVsDf2PhgG83xOk-9IFSVxh-o-ionsPiwx6N43jKz4BiuZX5aSHObbZFMRHWWSyrxIYyCTE263Ml6YtL92bywnTbAGU8yCoWdhtO0EhgGY8weZ5rL0bL0ROhKVVQEaqXIPZnUPnFg-TqCEWQAaicFtqLoG78Y_CCFOhBD9EKIXQi5WkvbHQ6gMsbtDvRekjkaf-FRddNVBVzX14PYo45DScSqdsTqUhEApWZwty3gV58llGTUi2TAP6ag7zL2hcl2k0TL57Q3Ruie65va75tZdQ_yAxoVbpk5RjDtid3BKJ7pUV3kvk1Sx0zjohI6MwIlE-nl0xFtFDzI-4MXhflyjHfs75dx2dYpo2v9XuxoccJk7XjR4z1K0j6F9VJy5IgoUoQ0KyHF66qF_doUFbcKtEhCtns2r6D231-ig7tUjnaq9oHcL1Fo-NBeeurAaNuz8qqXSo6KYcBWdYTFxemSGD5UWfKaTGZjsqzz1m941Pw3UjUZgk7GgsSQ1bWuhyUWd-SJqLqDm5vq5_H9z9yd_cGsr9VkQ_uimfQw3mYQi3kj84Tvsuakbrgk1_EDcL2khqZU0qCohHINrmLrgBjYtghYPxrQIPgm8A5zQGiIYMmbImJ3eMeScT-c9yIghHkO8Z5ALPl1oiJGFFZzVqZ3ZSnDPb82iNYuOedSaR8psWn43VhgvGAMJN-xD7d6dJj1LLNJR_Oil-JH1LOmX40WD539V_eda_8n-BQ)
