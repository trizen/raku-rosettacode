[1]: https://rosettacode.org/wiki/GSTrans_string_conversion

# [GSTrans string conversion][1]

```perl
# 20231105 Raku programming solution

sub GSTrans-encode(Str $str) {
   return [~] $str.encode('utf8').list.chrs.comb.map: -> $c { 
      my $i = $c.ord;
      die "Char value of $c, $i, is out of range" unless 0 <= $i <= 255;
      given ($i,$c) { 
         when 0 <= $i <= 31    { '|' ~ chr(64 + $i) } 
         when $c eq '"'        { '|"' }
         when $c eq '|'        { '||' }
         when $i == 127        { '|?' }
         when 128 <= $i <= 255 { '|!' ~ GSTrans-encode(chr($i - 128)) }
         default               { $c }
      }
   }
}

sub GSTrans-decode(Str $str) {
   my ($gotbar, $gotbang, $bangadd) = False, False, 0;

   my @result = gather for $str.comb -> $c {
      if $gotbang {
         if $c eq '|' {
            $bangadd = 128;
            $gotbar = True;
         } else {
            take $c.ord + 128;
         }
         $gotbang = False;
      } elsif $gotbar {
         given $c {
            when $c eq '?' { take 127 + $bangadd }
            when $c eq '!' { $gotbang = True }
            when $c eq '|' || $c eq '"' || $c eq '<' { take $c.ord + $bangadd }
            when $c eq '[' || $c eq '{' { take 27 + $bangadd } 
            when $c eq '\\' { take 28 + $bangadd } 
            when $c eq ']' || $c eq '}' { take 29 + $bangadd } 
            when $c eq '^' || $c eq '~' { take 30 + $bangadd }
            when $c eq '_' || $c eq '`' { take 31 + $bangadd }
            default { my $i = $c.uc.ord - 64 + $bangadd;
                      take $i >= 0 ?? $i !! $c.ord      }
         }
         $gotbar = False;
         $bangadd = 0;
      } elsif $c eq '|' {
         $gotbar = True
      } else {
         take $c.ord
      }
   }
   return Blob.new(@result).decode('utf8c8')
}

my @TESTS = <ALERT|G 'wert↑>;
my @RAND_TESTS = ^256 .roll(10).chrs.join xx 8;
my @DECODE_TESTS = < |LHello|G|J|M |m|j|@|e|!t|m|!|? abc|1de|5f >; 

for |@TESTS, |@RAND_TESTS -> $t {
   my $encoded = GSTrans-encode($t);
   my $decoded = GSTrans-decode($encoded);
   say "String $t encoded is: $encoded, decoded is: $decoded.";
   die unless $t ~~ $decoded;
}
for @DECODE_TESTS -> $enc {
    say "Encoded string $enc decoded is: ", GSTrans-decode($enc);
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jVZLb9pAEL5W_IoBIYEVsICUlIZXXjRVlbZS4FAprxq8gBNjt2s7D7Hm2HvPveTQ_qke-0s6612bNSERe7DXs983r52d9a_f1LgJHh__BP643Pj76osXDOG4P6CG45WJM3JNUuz7FPKeTzWYZwCAEj-gDpwtLiKpLlEF1NAoaLpteb4-mlJPH7mzoT4zvu1CuQP5EcyB03HMHiBvQRtlukvNppSaFoHc4dSgcGvYAQF3jIASIktgeeAGPpegXxOSg8CxiedBBVptrgqftXo9VjSxbokDRSTmR9rSKo67KS4opO0ql86hwAqwAHS6uPMatnBRg3CVhgGQ71DIFWIpp-FXuB7HUji2BocZaEO19kbFdZ_iqrVGKsoIl-X-rmwTdx9RZc7QNFWPScZGYPuQHnPuaoyK3mEmzGTUAjDJugLA7SvmJ64_NChuTzRxJjjjL8M0NdzZd4btkVL8qjQzkrdHicc9acPE8KeEwtgVqqNiietE-mSNE-2JTIqTHCtyHLELwBPbaKbXhMe4NKABUdZCIOjliibfuCGyPrEg0sqUzCb-yZBjVKQz8Z-qykV1KmGqNSHiwiqYCw94eWwtwwqfpWQ5RfGGx_gCHDPHmFLSy49WYjuJfgPzZ6qKeaJixXt4ln9-vuQ0NuRcqDbDJf_thvxLlb9I-NuVzUK-Uulfl_Tq8_T4HM7VDhiINJdBtB7JTNfucoidsaDTxj7W7fJpNhtvlSi9FwqVrtYppM5M5Un5rjtn6YOkMlKHSKmhdI-B5Ao5sN2h7pC7omwLmi4bTnSXjPA24Q2Jt41Brz_oo8XW_knvdMCOoXBHqP_vx89OM1o_3f90dBWDLmv1HdCpa9vFakUTd9G1azlwfw8NgT_qHX4-6iWMFrCT98S2XXbMPrCPwGbsmu0xwrI-TrOsC8ZwxKomYfUxdJqQyfC-xYRbJZwo9nkP85NGmRfNmWd3pV3nfa0Zg0TYKkgmIqYLqGc8QA6bsYVHHG3Eqi1vN7FTglhXJJUfei7i8xtW3pxIXyyS9SbmmUeUzguPBNXKTY2M96RJTzrBl1WDudK6CND7UPxgyP-M-H_jPw)
