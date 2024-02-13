[1]: https://rosettacode.org/wiki/Lyndon_word

# [Lyndon word][1]

```perl
# 20240211 Raku programming solution

sub nextword($n, $w, $alphabet) {
   my $x = ($w x ($n div $w.chars + 1)).substr(0, $n);
   while $x.Bool && $x.substr(*-1) eq $alphabet.substr(*-1) {
      $x.substr-rw(*-1) = ''
   }
   if $x.Bool {
      my $next_char_index  = ($alphabet.index($x.substr(*-1)) // 0) + 1;
      $x.substr-rw(*-1, 1) = $alphabet.substr($next_char_index, 1);
   }
   return $x;
}

.say for sub ($n, $alphabet) {
   my $w = $alphabet.substr(0, 1);
   return gather while $w.chars <= $n {
      take $w;
      last unless $w = nextword($n, $w, $alphabet);
   }
}(5, '01');
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=fVLLToQwFE1c8hVnQQaqwMDCxARn449MOlKEDBZti2Vi-BI3s9Cf8mtsedSMjpLQEO49j3t6394F3XfH40enyvjm84LIbgfOeqVbUYQ-j-Br89LmqaI7pghePQCPB_g9Ngh9jd6cHEX9YjqT-4oKiStkhCSGSSoRpgbOSW5huqobZpDJXds2WK3s59x1GWcE7Plb6aQwiprH9cdCT5UNgsAWB3vUpSNfENapHWdrnW1rXrAeo3GnM_4LT50QrNdIiR0k_0M6wqj-y-9PNduXO4eCqU5wQ5Z7g-clkh5QtgI29CnsM0HrczKp450pH6iqmFgiXm7i1kC5C0PRva0tIzVUKnS8YVJOKv_c-zzCEF5HCNIsIPm0MfPiLAv0BQ)
