[1]: https://rosettacode.org/wiki/Consistent_overhead_byte_stuffing

# [Consistent overhead byte stuffing][1]

```perl
# 20231102 Raku programming solution

sub encode($data where $data.Bool, $delim = 0x00) {
   my ($ins,$code,$addLastCode,@enc) = 0,1,True,-1;
   for $data.list -> $byte {
      if $byte != 0 { @enc.push($byte) andthen $code++ }
      $addLastCode = True;
      if $byte == 0 || $code == 255 {
         $addLastCode = False if $code == 255;
         @enc[$ins] = $code;
         $code = 1;
         $ins = @enc.elems;
         @enc.push(-1);
      }
   } 
   if $addLastCode {
      @enc[$ins] = $code;
      if $delim != 0 { @enc >>^=>> $delim } 
      @enc.push($delim)
   } else {
      if $delim != 0 { @enc >>^=>> $delim }
      @enc[$ins] = $delim
   }
   return @enc;
}

sub decode($data where $data.Bool, $delim = 0x00) {
   my $length = ( my @enc2 = $data.list[0..*-2] ).elems;
   if $delim != 0 { @enc2 >>^=>> $delim }
   my ($block,$code,@dec) = 0,255;
   for @enc2.kv -> $i,$byte {
      if $block != 0 {
         @dec.push($byte)
      } else {
         die "marker pointing beyond the end of the packet." if $i + $byte > $length;
         @dec.push(0) if $code != 255;
         $block = $code = $byte;
         last if $code == 0
      }
      $block--;
   }
   return @dec;
}

for ( [0x00], [0x00,0x00], [0x00,0x11,0x00], [0x11,0x22,0x00,0x33],
      [0x11,0x22,0x33,0x44], [0x11,0x00,0x00,0x00], # 0x01..0xfe, 0x00..0xfe, 
#      [0x02..0xff].append(0x00), [0x03..0xff].append(0x00, 0x01),
) { # https://crccalc.com/bytestuffing.php 
#   say encode( $_ )>>.&{.fmt("0x%x")}; 
   say decode(encode( $_ ))>>.&{.fmt("0x%x")}
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=nVTLjpswFF11w1fcydAKGuMAmZGqoqBRK3XVZXdRWjlgBhQCCEybKJMvmc0s2p9qf6Z-kToZumhZJPZ9nHPv9bEfv7dk0z89_ehZ5r35-eJX16-BVkmdUsdOCSPwLactBbnG7-q6RHxNy2ILC_B3vu_CwQKA7R4cu6g6ZItUZJM0_Ug69l5s7jieK8JRgD61PUVeEImcrG41bll0DLwY7PWeUQXIvyLThiueCwcQOLjpu9yRZhdIlbKcViA5p1M46kSTnfMKzugScyEwHx5UrtiFt7cn5ucYH0jZUZltxEd_wkVtSzGAFQ-WMYZT50Bg2ngsN8meaEm33QWYatQL3MEumzuCpZswyxvK_nsRIkOdmjFLiOPPizgePAr7jF55XMVMxQQO_wI4Wpb0WUNDLWV9W8mQyDpalpBfSv9HfnZJq3uWc7sjtgIxlHyDwJY-xq-9cAWuMfDRPsKxRqTC12WdbLTG73idStaDFISgZT7efJVyLtCIogWEZjOOnIOZ2h4O_Xzq_EsLCpMtaTe0haYuKlZU97Cm-7pKgV8GfndTqDO5bEiyoQxPJG0BU638eBhVNEbPJ3pS-dWlynXxi5OiJaIRUHJJnt0S31TvCcHzomcK4BVIBYghOrAUh7tC6h9dbILAsMhNGCLtm89XSHOdOedz_nNzY-Ro3AHqWugpwNjfZRRJbQ1r6_qE54fSmK0waRo-akdqUFU2H3FJoMBFFtcpZ8gZa7q3s1nSJgkpE5zU25mYYMf6LOMHiZu8UXQd2Q_PMNhfwI1j_OqAsy1zJv7u5W7iHiN5XUWcvi9m-Ei8dVRvvH7qhyf_Nw)
