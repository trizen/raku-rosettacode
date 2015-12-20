[1]: http://rosettacode.org/wiki/Vigenère_cipher

# [Vigenère cipher][1]

```perl
sub s2v ($s) { $s.uc.comb(/ <[ A..Z ]> /)».ord »-» 65 }
sub v2s (@v) { (@v »%» 26 »+» 65)».chr.join }
 
sub blacken ($red, $key) { v2s(s2v($red) »+» s2v($key)) }
sub redden  ($blk, $key) { v2s(s2v($blk) »-» s2v($key)) }
 
my $red = "Beware the Jabberwock, my son! The jaws that bite, the claws that catch!";
my $key = "Vigenere Cipher!!!";
 
say $red;
say my $black = blacken($red, $key);
say redden($black, $key);
```

#### Output:
```
Beware the Jabberwock, my son! The jaws that bite, the claws that catch!
WMCEEIKLGRPIFVMEUGXQPWQVIOIAVEYXUEKFKBTALVXTGAFXYEVKPAGY
BEWARETHEJABBERWOCKMYSONTHEJAWSTHATBITETHECLAWSTHATCATCH
```


This is a natural job for hyperoperators, which can vectorize any operator.
For infix operators the pointy end indicates which side to "dwim", repeating
elements on that side until the other side runs out. In particular, repeating
the key naturally falls out of this cyclic dwimmery, as does repeating the various constants to be applied with any of several operations to every element of the list. Factoring out the canonicalization and decanonicalization lets us see quite clearly that the only difference between encryption and decryptions is the sign of the vector addition/subtraction. Since hyperops are inherently parallelizable, this algorithm might run well in your GPU.