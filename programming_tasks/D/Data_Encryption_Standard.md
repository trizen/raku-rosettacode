[1]: https://rosettacode.org/wiki/Data_Encryption_Standard

# [Data Encryption Standard][1]

This is mainly a translation from the Phix entry, with an additional example on UTF-8. Regarding the many conversions among different number/string formats, small (and hopefully reusable ) helper routines are created to serve the purpose.
Update 20190323: After a bug fixed an example does behave correctly and is now in line with the results from the C, D, Kotlin and Phix entries. By the way it seems *.comb* handle "\r\n" inconsistently, why? [[1]](https://pastebin.com/d7dBpYkL)
Update 20190325: Thanks to SqrtNegInf for pointing out that the answer is already in the documentation.[[2]](https://docs.perl6.org/type/Str#routine_chomp), [[3]](https://docs.perl6.org/language/newline)

```raku
#!/usr/bin/env perl6
 
use v6.d;
use experimental :pack;
 
my \PC1 = <
   57 49 41 33 25 17  9        1 58 50 42 34 26 18
   10  2 59 51 43 35 27       19 11  3 60 52 44 36
   63 55 47 39 31 23 15        7 62 54 46 38 30 22
   14  6 61 53 45 37 29       21 13  5 28 20 12  4
>; # Permuted choice 1 (PC-1) - Parity Drop Table
 
my \PC2 = <
   14 17 11 24  1  5  3 28    15  6 21 10 23 19 12  4
   26  8 16  7 27 20 13  2    41 52 31 37 47 55 30 40
   51 45 33 48 44 49 39 56    34 53 46 42 50 36 29 32
>; # Permuted choice 2 (PC-2) - Key Compression Table
 
my \IP = <
   58 50 42 34 26 18 10  2    60 52 44 36 28 20 12  4
   62 54 46 38 30 22 14  6    64 56 48 40 32 24 16  8
   57 49 41 33 25 17  9  1    59 51 43 35 27 19 11  3
   61 53 45 37 29 21 13  5    63 55 47 39 31 23 15  7
>; # Initial permutation (IP)
 
my \IP2 = <
   40 8 48 16 56 24 64 32     39 7 47 15 55 23 63 31
   38 6 46 14 54 22 62 30     37 5 45 13 53 21 61 29
   36 4 44 12 52 20 60 28     35 3 43 11 51 19 59 27
   34 2 42 10 50 18 58 26     33 1 41  9 49 17 57 25
>; # Final permutation (IP⁻¹)
 
my \S = ( <
   14 4 13 1 2 15 11 8 3 10 6 12 5 9 0 7   0 15 7 4 14 2 13 1 10 6 12 11 9 5 3 8
   4 1 14 8 13 6 2 11 15 12 9 7 3 10 5 0   15 12 8 2 4 9 1 7 5 11 3 14 10 0 6 13
> , <
   15 1 8 14 6 11 3 4 9 7 2 13 12 0 5 10   3 13 4 7 15 2 8 14 12 0 1 10 6 9 11 5
   0 14 7 11 10 4 13 1 5 8 12 6 9 3 2 15   13 8 10 1 3 15 4 2 11 6 7 12 0 5 14 9
> , <
   10 0 9 14 6 3 15 5 1 13 12 7 11 4 2 8   13 7 0 9 3 4 6 10 2 8 5 14 12 11 15 1
   13 6 4 9 8 15 3 0 11 1 2 12 5 10 14 7   1 10 13 0 6 9 8 7 4 15 14 3 11 5 2 12
> , <
   7 13 14 3 0 6 9 10 1 2 8 5 11 12 4 15   13 8 11 5 6 15 0 3 4 7 2 12 1 10 14 9
   10 6 9 0 12 11 7 13 15 1 3 14 5 2 8 4   3 15 0 6 10 1 13 8 9 4 5 11 12 7 2 14
> , <
   2 12 4 1 7 10 11 6 8 5 3 15 13 0 14 9   14 11 2 12 4 7 13 1 5 0 15 10 3 9 8 6
   4 2 1 11 10 13 7 8 15 9 12 5 6 3 0 14   11 8 12 7 1 14 2 13 6 15 0 9 10 4 5 3
> , <
   12 1 10 15 9 2 6 8 0 13 3 4 14 7 5 11   10 15 4 2 7 12 9 5 6 1 13 14 0 11 3 8
   9 14 15 5 2 8 12 3 7 0 4 10 1 13 11 6   4 3 2 12 9 5 15 10 11 14 1 7 6 0 8 13
> , <
   4 11 2 14 15 0 8 13 3 12 9 7 5 10 6 1   13 0 11 7 4 9 1 10 14 3 5 12 2 15 8 6
   1 4 11 13 12 3 7 14 10 15 6 8 0 5 9 2   6 11 13 8 1 4 10 7 9 5 0 15 14 2 3 12
> , <
   13 2 8 4 6 15 11 1 10 9 3 14 5 0 12 7   1 15 13 8 10 3 7 4 12 5 6 11 0 14 9 2
   7 11 4 1 9 12 14 2 0 6 10 13 15 3 5 8   2 1 14 7 4 10 8 13 15 12 9 0 3 5 6 11
> ); # 8 Substitution boxes, each replaces a 6-bit input with a 4-bit output
 
my \P = <
   16 7 20 21   29 12 28 17     1 15 23 26     5 18 31 10
    2 8 24 14   32 27  3  9    19 13 30  6    22 11  4 25
>; # Permutation (P), shuffles the bits of a 32-bit half-block
 
# Expansion function (E), expand 32-bit half-block to 48 bits
my \E = flat 32,1..5,4..9,8..13,12..17,16..21,20..25,24..29,28..32,1;
 
my \SHIFTS = < 1 1 2 2 2 2 2 2 1 2 2 2 2 2 2 1 >; # schedule of left shifts
 
## Helper subs
 
# convert iso-8859-1 to hexadecimals
sub b2h (\b) { [~] map { .encode('iso-8859-1').unpack('H*') }, b.comb };
 
# convert UTF8s to bytes
sub u2b (\u) { [~] map { .chr }, @( [~] map { .encode('utf8') }, u.comb) };
 
# convert hexadecimals to UTF-8
sub h2u (\h) { pack("H" x h.chars/2, h ~~ m:g/../).decode('utf8') };
 
# convert quadbits to hex
sub q2h (\q) { [~] map { :2($_.Str).fmt('%X') }, q ~~ m:g/..../ };
 
# convert every two quadbits to bytes
sub q2b (\q) { map { :2($_.Str) }, q ~~ m:g/. ** 8/ };
 
# trun a 16 digit hexadecimal str to a 64 bits list
sub h2bits (\h) { ([~] map { :16($_).base(2).fmt('%04s') }, h.comb).split("")[1..64] };
 
sub infix:<⥀>(\a is copy, \b) { a.append: a.shift for ^b ; a } # XOR addition
 
# convert hexadecimals to bytes
sub h2bytes (\h) { [~] map { :16($_.Str).chr }, h ~~ m:g/../ };
 
# s is 16 digit hexadecimal str, M is a permuation matrix/vector
sub map64(\s, \M) { my \b = h2bits s; map { b[$_-1] }, M; }
 
## Core subs
 
sub get_subkeys(Str \key --> Seq) { # return a Seq with 16 bit vectors
   my \Kₚ = map64 key, PC1; # drop parity bits
   my @C = Kₚ[0..27] ; my @D = Kₚ[28..55]; # perform bits rotation next
   my \CD = map { [ flat @C ⥀= SHIFTS[$_], @D ⥀= SHIFTS[$_] ]}, ^16;
   return map { map { CD[$_][PC2[$^a]-1] }, ^48 }, ^16; # key compression rounds
}
 
sub ƒ (List \R is copy, Seq \Kₙ is copy --> Seq) {
   my @er = map { Kₙ[$_] +^ R[E[$_]-1] }, ^48;
   my @sr = flat map { # Sₙ(Bₙ) loop, process @er six bits at a time
      ((S.[$_][([~] @er[$_*6,$_*6+5]).parse-base(2)*16+([~]
         @er[$_*6+1 .. $_*6+4]).parse-base(2)]).fmt('%04b').split(""))[1..4]
   }, ^8;
   return map { @sr[$_-1] }, P;
}
 
sub process_block(Str \message, \K is copy --> Str) { # return 8 quadbits
   my \mp = map64 (b2h message) , IP; # turn message to hex then map to bits
   my @L = mp[0..31]; my @R = mp[32..63];
   my (@Lₙ , @Rₙ); # then apply 16 iterations with function ƒ
   { @Lₙ = @R; @Rₙ = @L Z+^ ƒ @R, K[$_]; @L = @Lₙ; @R = @Rₙ } for ^16;
   my \res = flat @R, @L; # reverse and join the final L₁₆ and R₁₆
   return [~] map { res[$_-1] }, IP2 ; # inverse of the initial permutation
}
 
sub des(Str \key, Str $msg is copy, Bool \DECODE --> Str) { # return hexdecimal
   my @K; my \length = $msg.encode('iso-8859-1').bytes;
   if ( DECODE and length % 8 ) { # early exit, avoid the subkeys computation
      die "Message must be in multiples of 8 bytes"
   } else { 
      @K = DECODE ?? reverse get_subkeys key !! get_subkeys key  
   }
   {  my \P = 8 - length % 8; # number of pad bytes
      $msg ~= P.chr x P ; # CMS style padding as per RFC 1423 & RFC 5652
   } unless DECODE;
 
   my $quad ~= process_block substr($msg,$_,8), @K for 
      0, 8 … $msg.encode('iso-8859-1').bytes-8;
 
   {  my @decrypt = q2b $quad; # quadbits to a byte code point list
      @decrypt.pop xx @decrypt.tail; # remove padding
      return b2h ( [~] map { .chr } , @decrypt )
   } if DECODE ;
 
   return q2h $quad
}
 
say "Encryption examples: ";
say des "133457799BBCDFF1", h2bytes("0123456789ABCDEF"), False;
say des "0E329232EA6D0D73", h2bytes("8787878787878787"), False;
say des "0E329232EA6D0D73", "Your lips are smoother than vaseline", False;
say des "0E329232EA6D0D73", "Your lips are smoother than vaseline\r\n", False;
say des "0E329232EA6D0D73", u2b("BMP: こんにちは ; Astral plane: 𝒳𝒴𝒵"), False;
 
say "Decryption examples: ";
say des "133457799BBCDFF1", h2bytes("85E813540F0AB405FDF2E174492922F8"), True;
say des "0E329232EA6D0D73", h2bytes("0000000000000000A913F4CB0BD30F97"), True;
say h2bytes des "0E329232EA6D0D73", h2bytes("C0999FDDE378D7ED727DA00BCA5A84EE47F269A4D6438190D9D52F78F535849980A2E7453703513E"), True;
say h2bytes des "0E329232EA6D0D73", h2bytes("C0999FDDE378D7ED727DA00BCA5A84EE47F269A4D6438190D9D52F78F53584997F922CCB5B068D99"), True;
say h2u des "0E329232EA6D0D73", h2bytes("C040FB6A6E72D7C36D60CA9B9A35EB38D3194468AD808103C28E33AEF0B268D0E0366C160B028DDACF340003DCA8969343EBBD289DB94774"), True;
```

#### Output:
```
Encryption examples:
85E813540F0AB405FDF2E174492922F8
0000000000000000A913F4CB0BD30F97
C0999FDDE378D7ED727DA00BCA5A84EE47F269A4D6438190D9D52F78F535849980A2E7453703513E
C0999FDDE378D7ED727DA00BCA5A84EE47F269A4D6438190D9D52F78F53584997F922CCB5B068D99
C040FB6A6E72D7C36D60CA9B9A35EB38D3194468AD808103C28E33AEF0B268D0E0366C160B028DDACF340003DCA8969343EBBD289DB94774
Decryption examples:
0123456789abcdef
8787878787878787
Your lips are smoother than vaseline
Your lips are smoother than vaseline

BMP: こんにちは ; Astral plane: 𝒳𝒴𝒵
```