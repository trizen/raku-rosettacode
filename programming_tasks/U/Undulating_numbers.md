[1]: https://rosettacode.org/wiki/Undulating_numbers

# [Undulating numbers][1]

```perl
# 20230602 Raku programming solution

sub undulating ($base, \n) {
   my \limit = 2**(my \mpow = 53) - 1;
   my (\bsquare,@u3,@u4) = $base*$base; 
   for 1..^$base X 0..^$base -> (\a,\b) {
      next if b == a;
      @u3.push(a * bsquare + b * $base + a);
      @u4.push((my \v = a * $base + b) * bsquare + v)
   }
   say "\nAll 3 digit undulating numbers in base $base:";
   .fmt('%3d').say for @u3.rotor: 9;
   say "\nAll 4 digit undulating numbers in base $base:";
   .fmt('%4d').say for @u4.rotor: 9; 
   say "\nAll 3 digit undulating numbers which are primes in base $base:";
   my @primes = @u3.grep: *.is-prime; 
    .fmt('%3d').say for @primes.rotor: 10, :partial;
   my \unc = (my @un = @u3.append: @u4).elems;
   my ($j, $done) = 0, False;
   loop {
      for 0..^unc {
         my  \u = @un[$j * unc + $_] * bsquare + @un[$j * unc + $_] % bsquare;
	 u > limit ?? ( $done = True and last ) !! ( @un.push: u );
      }
      $done ?? ( last ) !! $j++ 
   }
   say "\nThe {n} undulating number in $base $base is: @un[n-1]";
   say "or expressed in base $base : {@un[n-1].base($base)}" unless $base == 10;
   say "\nTotal number of undulating numbers in base $base < 2**{mpow} = {+@un}";
   say "of which the largest is: ", @un[*-1];
   say "or expressed in base $base : {@un[*-1].base($base)}" unless $base == 10;
}

undulating $_, 600 for 10, 7;
```


You may [Try it online!](https://tio.run/##nVTbbtpAEH0uX3GCHMU2xoJCU9UuCXnpF/BQqaTROl7AaL12fclFyN9OZ9c2mBSpaZHwZXfmnDMzx5vyTFzv93kZoJRhKVgRyTVMI2A5d7CUFnY9APErliKKowIzfLRtU73HafJMr58mFoYY@02YuQzyXyXLuDMvJ/SfWhSj4Wx99aECV0mGsev@1Ev4jtHheXhDEMxZBg0z/SR/KRCtEGA2A/ObVYJ30zLfmAw2Gk4MKMiu6eiZWcfgaR2slT@RJNaJI64uxJOlsip1ydkr@kt5JwQmCKM1NaDTJlnGAc9yRBIaSeN5fU3qruLCvLqchFeWq1BUxUpylhRJ5uGL/wZ@@l/w01P46REe75f/vIkeN1C1p1kU8/OE1Ld5sz3Thawznnqw3Sgf6vWa8XzddWIrbjxy4KUsKyImWuxlKR8JWI1nXsqGgqUpl6GnCrNcLnicH1xmbB0YYSK5shfhfWOCvKV2RZKkB@soduUthd6u1QjEqFnkD2NL01cBAxgP9ydOOLN92W77vQ8ocYP6s7i9hVkLItRFVnIwGUKwvICFiwvaJCxtQY@yDr6smnudqUGOOcZ2MMBbKy42HDtZ/TlHNTXjODZEuaf1y@H4vn90GzWEv6QZz3Meng4aHnZtgqsW6lPAqvpEJiihCaOPcDzq2neRFEy0KpLVXy2Mr@oM2anzo6Ju7QbEWnUlrhpLFlSrYNmaU0dUOX1HV2STwH8pyH5fQVWv11FuPDi4Ho3qk4oM9tnf738D)
