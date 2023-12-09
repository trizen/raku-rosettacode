[1]: https://rosettacode.org/wiki/Check_if_a_polygon_overlaps_with_a_rectangle

# [Check if a polygon overlaps with a rectangle][1]

```perl
# 20230810 Raku programming solution

class Vector2 { has ( $.x, $.y );
   method dot ( \other ) { self.x * other.x + self.y * other.y }
}; 

class Rectangle { has ( $.x, $.y, $.w, $.h ) }

class Projection { has ( $.min, $.max ) };

sub getAxes(\poly) {
   return poly.append(poly[0]).rotor(2=>-1).map: -> (\vertex1,\vertex2) {
      my \vector1 = Vector2.new: x => vertex1[0], y => vertex1[1];
      my \vector2 = Vector2.new: x => vertex2[0], y => vertex2[1];
      my \edge    = Vector2.new: x => vector1.x - vector2.x, 
                                 y => vector1.y - vector2.y;
      $_ = Vector2.new: x => -edge.y, y => edge.x
   }
}

sub projectOntoAxis(\poly, \axis) {
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

sub projectionsOverlap(\proj1, \proj2) {
   return ! ( proj1.max < proj2.min or proj2.max < proj1.min )
}

sub rectToPolygon( \r ) {
    return [(r.x, r.y), (r.x, r.y+r.h), (r.x+r.w, r.y+r.h), (r.x+r.w, r.y)]
}

sub polygonOverlapsRect(\poly1, \rect) {
   my \poly2 = rectToPolygon rect;
   my (\axes1,\axes2) := (poly1,poly2).map: { getAxes $_ };
   for (axes1, axes2) -> \axes {
     for axes -> \axis {
         my (\proj1,\proj2) := (poly1,poly2).map: { projectOntoAxis $_, axis }
         return False unless projectionsOverlap(proj1, proj2) 
      }
   }
   return True
}

my \poly = [ <0 0>, <0 2>, <1 4>, <2 2>, <2 0> ]; 
my \rect1 = Rectangle.new: :4x, :0y, :2h, :2w;
my \rect2 = Rectangle.new: :1x, :0y, :8h, :2w;
say "poly  = ", poly;
say "rect1 = ", rectToPolygon(rect1);
say "rect2 = ", rectToPolygon(rect2);
say();
say "poly and rect1 overlap? ", polygonOverlapsRect(poly, rect1);
say "poly and rect2 overlap? ", polygonOverlapsRect(poly, rect2);
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=lVXNbtswDD4OyFNwRQ_26hi20EPhNBl62bXFUOzSBIPXKHE21w5kp7ER5El26aF7qO1pRoqS6zg_w3ywJIrkR32kqJ-vKv6xenn5tSpn_as_734_pnFRwBf5WOZKwAaSuAAHzv3Kw18N7qAHAE-yTPIpTPMS98Z5mUgFLioXMp35FXwALcLZBYvqRlTDtrcdQK_HOJ8RJ87mqdxDot-afgl63lr9O5V_R5NFnrUMnhYZKT7FFakOer1i9Q3msrypZOGMl3laY2wUtpLlSmVAEj9eLmU2dWj-EExcX-V4YEcMR_3QRVfLCPojcMbPUpWyCj0zEcYTcVADCommEIaWMD-T6wgqGI7AWKJzD-q2IJwM9lyIEy5E14XouJDTuaT5YRc6QkxF38wFEWysT3x127puWdcW-vzrQcQ-hUP50x70oiILzDtnZsk5vM3K_KZamAx5MI5xYehlYuisAWKYHA3edggzOEFZ0KUssJSh_TkVCvOlV4sMZwTuYz1H5pyB1p7lSqNTLZiADuT_RCDdOFqZmy-eZbaHi2W9TlCO5F5zbBsbIoq2_0ycNR7xMTc8GOMt5-HtJrxdJ45cA400oAfakP10Uof6xS0eKI2XmD2UhZg-GsXuRXuP11Nv67t5reeCrisQr7xoNkK94VoohTj3-R2SP88zbDK6wejTG-cPjqJKxpbietDML5SfmDVO10dl7qQ5E0OY8xTUkbgk6VAURasmSUxXdSc4vbLF5WAZywL7BQ1IRzQEh51pW9NaNrY96bw0peawLRhbKjqa2pIjFb3mjUWzARabc2FTcQy7cwExBk_XIZcGf4bkT3FaSFhlqcTmeyD7JvkG0Jjvldm9Wkmi21KIDD7AdQDByKNB0BDCJQ2CVwL3YILvBJkQv9Rjm8eCizW6xJRHAbaOSCT0Ww8adXFAPWzUr6x6EddwpiNC_TNP14KRWlCU7pai3nBbWuKYlmAtx20DxdkU2HfOHH60wN0i5La4A7fjQfyHBwyFX3jz0NsH_y8)
