[1]: https://rosettacode.org/wiki/Factorize_string_into_Lyndon_words

# [Factorize string into Lyndon words][1]

```perl
# 20240213 Raku programming solution

sub chenfoxlyndonfactorization(Str $s) {
   my ($n, $i, @factorization) = $s.chars, 1;
   while $i <= $n {
      my ($j, $k) = $i X+ (1, 0);
      while $j <= $n && substr($s, $k-1, 1) <= substr($s, $j-1, 1) {
         substr($s, $k-1, 1) < substr($s, $j++ -1, 1) ?? ( $k = $i ) !! $k++;
      }
      while $i <= $k {
         @factorization.push: substr($s, $i-1, $j-$k);
         $i += $j - $k
      }
   }
   die unless $s eq [~] @factorization;
   return @factorization
}

my $m = "0";
for ^7 { $m ~= $m.trans('0' => '1', '1' => '0') }
say chenfoxlyndonfactorization($m);
```


Same as Julia example.



You may [Attempt This Online!](https://ato.pxeger.com/run?1=fVJNToNAFE5ccoqvDSlMoA2sNGJt7-DGxGhCKYRpy6AzQ7Q27UXcdKGX8jS-4SeWxshiwrz3_b3J-_iU8bo6Hr8qnY2vvi_mqlogyVORlW-brViWIosTXUr-HmteCvdOS9iKYWcBKLZwbeHD5j7mPRzDlGCTJI-l8hFGBv2a801KWNxQTzQKnciKRNY1iePegxv6CFjUIlriqiWORqCQSkvXVoY2JnDITPO0vGrLnQ19f7L6JM9D25jN4BKsicQwGNDF87pI-360Zqb1qVn_PSbPlcqve17cGFFMmjv6pZGUNzWzjknv1Kw-ljxFJTapUvS6SF_wcHg8c6q1ZKorKc461t6y6LHtgmYaBsPIykqJp0vsTOlApsVEy1go1wkcTG_hhI5vjvo_cBhFUPH2v-WwCxY1i9TuU7dXPw)
