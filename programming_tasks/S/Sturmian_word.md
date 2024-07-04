[1]: https://rosettacode.org/wiki/Sturmian_word

# [Sturmian word][1]

```perl
# 20240215 Raku programming solution

sub chenfoxlyndonfactorization(Str $s) {
 sub sturmian-word($m, $n) {
   return sturmian-word($n, $m).trans('0' => '1', '1' => '0') if $m > $n; 
   my ($res, $k, $prev) = '', 1, 0;
   while ($k * $m) % $n > 0 {
      my $curr = ($k * $m) div $n;
      $res ~= $prev == $curr ?? '0' !! '10';
      $prev = $curr;
      $k++;
   }
   return $res;
}

sub fib-word($n) {
   my ($Sn_1, $Sn) = '0', '01';
   for 2..$n { ($Sn, $Sn_1) = ($Sn~$Sn_1, $Sn) }
   return $Sn;
}

my $fib = fib-word(7);
my $sturmian = sturmian-word(13, 21);
say "{$sturmian} <== 13/21" if $fib.substr(0, $sturmian.chars) eq $sturmian;

sub cfck($a, $b, $m, $n, $k) {
   my (@p, @q) := [0, 1], [1, 0]; 
   my $r = (sqrt($a) * $b + $m) / $n;
   for ^$k {
      my $whole = $r.Int;
      my ($pn, $qn) = $whole * @p[*-1] + @p[*-2], $whole * @q[*-1] + @q[*-2];
      @p.push($pn);
      @q.push($qn);
      $r = 1/($r - $whole);
   }
   return [@p[*-1], @q[*-1]];
}

my $cfck-result = cfck(5, 1, -1, 2, 8);
say "{sturmian-word($cfck-result[0], $cfck-result[1])} <== 1/phi (8th convergent golden ratio)";
```


Same as Julia example.



You may [Attempt This Online!](https://ato.pxeger.com/run?1=ZVRNb5tAEJWqnvwrJhYRrA2YdVTVKiXxtWcfLTfCGAKyvcACtiLL-SO55ND-qf6azCyfSQ_Yq9k3b2beG3j9I_199fb2tyoja_Hv65ei2kJRVvKY-MI6p3JnaEcTNMHgMgIAGeKd-IwQiDgyu5S-KAzd0cG7B53rJv2os6MzSCIEwT1yuUBUx2cwNBkWmLvHJ5PhiYEHOqZxExyXMOc4OYQI28OEKsAtZiOFUzdTk2hBJSUm9qhdcqIqDYRqwItXVwDPaxIeHqgtuLnBJh29A9egGtMF99OpOl8HEhCtO7qORqRYlGxbKRqh1HQr8Yij4J8azCFBHF7XilIJc9vGcS4KqGCPnKlBVuJlmPuh7EqoqjQ4VkV4V_s7c1W4NQfvPvrE70yYc0QV_jOMLx3wCj9RFn43m_OxsgkpbRyrKKXhmD2hHcS-LBiEeR9zawGCKNgbmo_gLe0CbQz5OhBjmZmwzBn88GCNpHxjwpp83nTboCkXi1yWyMTIyy1MlaGz1k5S7Tf6PLT_HKe4I2iZtH-J0u1vDC2jJnKlfgObwDJbTyy-QWZ1mmMf_V3e3eXqrmVbZnZWFTExsi6WN7G8j6kR-Az3GqyGlv23OeumBbOtt-kcJRktXKzqUCKREvWbeh0sfOYmLDrzPr2Bg8S1QyMNA3zDGotnWZyAsShjCFJxCuVTKEp4Sg-7UID0yyRlY7f-FDRfhPbL8A4)
